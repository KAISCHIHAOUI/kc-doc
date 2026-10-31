---
title: "Fiabilité d'une application Spring Boot : guide pratique"
description: "Comment construire une application Spring Boot résiliente avec Resilience4j, Actuator et Kubernetes : détection, absorption et récupération automatique des pannes."
date: 2026-10-31
tags:
  - spring-boot
  - resilience
  - resilience4j
  - kubernetes
  - reliability
  - java
categories:
  - Backend
  - DevOps
draft: false
---

# Fiabilité d'une application Spring Boot : guide pratique

## Introduction

La **fiabilité** d'un système est sa capacité à fonctionner correctement dans le temps, même en présence de pannes partielles. Dans un contexte Spring Boot, cela se traduit par trois piliers :

1. **Détecter** les pannes rapidement (health checks)
2. **Absorber** les pannes transitoires (retry, circuit breaker)
3. **Récupérer** automatiquement (graceful shutdown, orchestrateur)

---

## 1. Détection : les health checks et les probes

### Pourquoi séparer liveness et readiness ?

| Probe | Question | Action en cas d'échec |
|---|---|---|
| **Liveness** | Le processus est-il fondamentalement cassé ? | Redémarrer le conteneur |
| **Readiness** | L'instance peut-elle servir du trafic ? | Retirer du load balancer (pas de redémarrage) |
| **Startup** | L'application a-t-elle fini de démarrer ? | Attendre (ne redémarre pas) |

>  **Règle d'or** : la liveness ne doit **jamais** dépendre d'un système externe (DB, API). Sinon, une panne DB → Kubernetes redémarre tous les pods → cascade de pannes.

### Configuration Spring Boot

```yaml
# application.yml
management:
  endpoint:
    health:
      probes:
        enabled: true
      show-details: always
  health:
    livenessstate:
      enabled: true
    readinessstate:
      enabled: true
  endpoints:
    web:
      exposure:
        include: health,info,prometheus
```

Cela expose :
- `GET /actuator/health/liveness` → `{ "status": "UP" }`
- `GET /actuator/health/readiness` → `{ "status": "READY" }` ou `{ "status": "REFUSING_TRAFFIC" }`

### Manifeste Kubernetes

```yaml
apiVersion: apps/v1
kind: Deployment
spec:
  template:
    spec:
      containers:
        - name: payment-service
          image: payment-service:1.0
          ports:
            - containerPort: 8080
          startupProbe:
            httpGet:
              path: /actuator/health/liveness
              port: 8080
            failureThreshold: 30
            periodSeconds: 10
          livenessProbe:
            httpGet:
              path: /actuator/health/liveness
              port: 8080
            periodSeconds: 10
            failureThreshold: 3
            timeoutSeconds: 5
          readinessProbe:
            httpGet:
              path: /actuator/health/readiness
              port: 8080
            periodSeconds: 5
            failureThreshold: 3
            timeoutSeconds: 3
```

### Custom Health Indicator

```java
@Component
public class PaymentGatewayHealthIndicator implements HealthIndicator {

    private final PaymentGatewayClient client;

    public PaymentGatewayHealthIndicator(PaymentGatewayClient client) {
        this.client = client;
    }

    @Override
    public Health health() {
        try {
            boolean reachable = client.ping();
            return reachable
                ? Health.up().withDetail("gateway", "reachable").build()
                : Health.down().withDetail("gateway", "unreachable").build();
        } catch (Exception e) {
            return Health.down(e).withDetail("gateway", "error").build();
        }
    }
}
```

Ce composant alimente le **readiness** (pas la liveness) : si la gateway de paiement est down, le pod est retiré du traffic sans être redémarré.

---

## 2. Absorption : Circuit Breaker avec Resilience4j

### Le problème

Un service externe (ex. API de paiement) devient lent ou indisponible. Sans protection, chaque requête attend un timeout → les threads s'accumulent → votre application s'effondre en cascade.

### La solution : le circuit breaker

```
CLOSED (normal)
   │  ≥ 50% d'échecs sur 10 appels
   ▼
OPEN (tous les appels échouent immédiatement → fallback)
   │  après 30 s
   ▼
HALF_OPEN (3 appels de test autorisés)
   │  suffisent de succès → CLOSED
   │  échec → OPEN
```

### Configuration

```yaml
resilience4j:
  circuitbreaker:
    instances:
      paymentService:
        sliding-window-type: COUNT_BASED
        sliding-window-size: 10
        minimum-number-of-calls: 5
        failure-rate-threshold: 50
        wait-duration-in-open-state: 30s
        permitted-number-of-calls-in-half-open-state: 3
        automatic-transition-from-open-to-half-open-enabled: true
        record-exceptions:
          - java.io.IOException
          - java.util.concurrent.TimeoutException
          - org.springframework.web.client.HttpServerErrorException
        ignore-exceptions:
          - com.myapp.exception.BusinessException
```

### Code Java

```java
@Service
public class PaymentService {

    private final RestClient restClient;

    public PaymentService(RestClient.Builder builder) {
        this.restClient = builder.baseUrl("http://payment-gateway").build();
    }

    @CircuitBreaker(name = "paymentService", fallbackMethod = "paymentFallback")
    @Retry(name = "paymentService")
    public PaymentResponse processPayment(PaymentRequest request) {
        return restClient.post()
            .uri("/api/charges")
            .body(request)
            .retrieve()
            .body(PaymentResponse.class);
    }

    private PaymentResponse paymentFallback(PaymentRequest request, Throwable t) {
        log.warn("Circuit breaker OPEN pour paymentService, fallback activé");
        return PaymentResponse.pending(request.getId());
    }
}
```

### États du circuit en production

| État | Comportement | Impact utilisateur |
|---|---|---|
| CLOSED | Appels normaux | Aucun |
| OPEN | Rejet immédiat → fallback | Réponse dégradée |
| HALF_OPEN | 3 appels de test | Quelques utilisateurs en test |

---

## 3. Absorption : Retry avec backoff exponentiel

### Quand utiliser le retry ?

- OUI : Pannes transitoires : timeout réseau, 503, connection reset
- NON : Erreurs de validation (400), opérations non-idempotentes sans clé d'idempotence

### Configuration

```yaml
resilience4j:
  retry:
    instances:
      paymentService:
        max-attempts: 3
        wait-duration: 500ms
        enable-exponential-backoff: true
        exponential-backoff-multiplier: 2
        enable-randomized-wait: true
        randomized-wait-factor: 0.5
        retry-exceptions:
          - java.io.IOException
          - java.net.SocketTimeoutException
          - org.springframework.web.client.HttpServerErrorException
        ignore-exceptions:
          - org.springframework.web.client.HttpClientErrorException
```

### Flux en cas de panne transitoire

```
Appel 1 → timeout (500ms)
Appel 2 → timeout (1s)
Appel 3 → timeout (2s)
→ 3 échecs comptés pour le CB
→ Si ≥ 50% sur 10 appels → CB OPEN → fallback
```

### Flux en cas de panne prolongée

```
CB déjà OPEN → CallNotPermittedException → fallback immédiat
→ Zéro appel réseau, zéro latence
```

---

## 4. Récupération : Graceful Shutdown

### Le problème

Sans graceful shutdown, un redémarrage (deploy, scale-down) coupe les requêtes en cours → 502 pour l'utilisateur.

### Configuration

```yaml
server:
  shutdown: graceful

spring:
  lifecycle:
    timeout-per-shutdown-phase: 30s
```

### Ce qui se passe

1. Le serveur **arrête d'accepter** de nouvelles connexions
2. Les requêtes en cours terminent (jusqu'au timeout)
3. Le processus se termine proprement

### En Kubernetes

```
SIGTERM → readiness DOWN → traffic drain → requêtes en cours finissent → exit 0
```

---

## 5. Observabilité : Micrometer + Prometheus

### Métriques exposées

| Métrique | Type | Description |
|---|---|---|
| `resilience4j_circuitbreaker_state` | Gauge | État (0=CLOSED, 1=OPEN, 2=HALF_OPEN) |
| `resilience4j_circuitbreaker_number_of_calls` | Counter | Appels totaux |
| `resilience4j_circuitbreaker_number_of_failed_calls` | Counter | Appels échoués |
| `resilience4j_retry_number_of_calls` | Counter | Retries effectués |
| `resilience4j_ratelimiter_available_permits` | Gauge | Permis restants |

### Alerting Prometheus

```yaml
groups:
  - name: circuit-breaker
    rules:
      - alert: CircuitBreakerOpen
        expr: resilience4j_circuitbreaker_state{state="OPEN"} == 1
        for: 30s
        labels:
          severity: warning
        annotations:
          summary: "Circuit breaker {{ $labels.name }} est OPEN"
          description: "Le service {{ $labels.name }} est en fallback depuis plus de 30s"
```

---

## 6. Architecture complète

```
┌─────────────────────────────────────────────────────────────┐
│                    Kubernetes / Docker                        │
│                                                              │
│  ┌──────────┐   ┌──────────────────────────────────────┐    │
│  │  Pod     │   │  Spring Boot App                      │    │
│  │          │   │                                        │    │
│  │  startup │──▶│  /actuator/health/liveness            │    │
│  │  probe   │   │  /actuator/health/readiness           │    │
│  │          │   │                                        │    │
│  │  liveness│──▶│  @CircuitBreaker("paymentService")    │    │
│  │  probe   │   │    @Retry("paymentService")           │    │
│  │          │   │      → PaymentGateway API             │    │
│  │  readiness│  │                                        │    │
│  │  probe   │──▶│  Micrometer → Prometheus → Grafana    │    │
│  └──────────┘   └──────────────────────────────────────┘    │
│                                                              │
│  Graceful shutdown: 30s drain avant kill                    │
└─────────────────────────────────────────────────────────────┘
```

---

## 7. Checklist fiabilité Spring Boot

| Pilier | Outil | Impact MTTR |
|---|---|---|
| **Détection** | Actuator probes + Micrometer/Prometheus | Détection en secondes |
| **Absorption transitoire** | `@Retry` + backoff exponentiel | Évite l'incident |
| **Absorption prolongée** | `@CircuitBreaker` + fallback | Évite la cascade |
| **Protection charge** | `@RateLimiter` / `@Bulkhead` | Évite la saturation |
| **Récupération** | Graceful shutdown + K8s restart | Redémarrage propre en < 1 min |
| **Récupération rapide** | GraalVM native / Project CRaC | Démarrage en ~100 ms |

---

## Dépendances Maven

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-actuator</artifactId>
    </dependency>
    <dependency>
        <groupId>io.github.resilience4j</groupId>
        <artifactId>resilience4j-spring-boot3</artifactId>
        <version>2.2.0</version>
    </dependency>
    <dependency>
        <groupId>io.micrometer</groupId>
        <artifactId>micrometer-registry-prometheus</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-aop</artifactId>
    </dependency>
</dependencies>
```

---

## Conclusion

La fiabilité n'est pas une fonctionnalité qu'on "ajoute" — c'est une **couche transversale** qui s'applique à chaque point de contact avec l'extérieur. Dans Spring Boot, les briques existent nativement (Actuator, lifecycle) et s'enrichissent avec Resilience4j pour les patterns de résilience. La clé est de les combiner de façon cohérente : **détecter vite, absorber intelligemment, récupérer automatiquement**.   
