# Monitoring vs Observabilité : Comprendre la différence pour mieux gérer vos systèmes

## Introduction

Dans le monde de l'ingénierie logicielle et des opérations IT, deux concepts reviennent constamment : le **monitoring** et l'**observabilité**. Souvent utilisés de manière interchangeable, ils désignent pourtant des approches fondamentalement différentes de la gestion des systèmes. Comprendre cette distinction est essentiel pour construire une infrastructure fiable et résiliente.

---

## Qu'est-ce que le Monitoring ?

Le monitoring est une pratique **réactive** qui consiste à surveiller un ensemble prédéfini de métriques pour vérifier que les systèmes fonctionnent correctement. C'est la première ligne de défense de toute équipe d'opérations.

### Comment ça fonctionne ?

Le monitoring repose sur des **alertes basées sur des seuils**. Vous définissez à l'avance ce que vous considérez comme "normal" et vous configurez des alertes quand ces valeurs sortent de la plage attendue.

**Exemples concrets :**
- "M'alerter si le taux d'erreur dépasse 5%"
- "M'alerter si le temps de réponse dépasse 2 secondes"
- "M'alerter si le serveur ne répond plus au ping"

### Les trois questions classiques du monitoring (les "Golden Signals")

1. **Latence** – Le service répond-il assez vite ?
2. **Trafic** – Quelle est la charge demandée au système ?
3. **Erreurs** – Le taux d'échec des requêtes est-il acceptable ?
4. **Saturation** – Le système est-il proche de ses limites ?

### Limites du monitoring

Le principal problème du monitoring est qu'il ne peut détecter que **ce que vous avez prévu de surveiller**. Si un problème survient dans une zone non couverte par vos dashboards ou vos alertes, vous ne le verrez tout simplement pas.

> **Analogie :** Le monitoring, c'est comme le tableau de bord d'une voiture. Il vous indique que le moteur chauffe, mais il ne vous dit pas *pourquoi* il chauffe.

---

## Qu'est-ce que l'Observabilité ?

L'observabilité est une approche **exploratoire et proactive** qui permet de comprendre l'état interne d'un système à partir de ses sorties externes, même face à des problèmes inédits. Elle ne se contente pas de dire *qu'il y a un problème*, elle permet de comprendre *pourquoi*.

### Les trois piliers de l'observabilité

| Pilier | Description | Exemple d'usage |
|--------|-------------|-----------------|
| **Logs** | Journaux textuels ou structurés des événements | "Erreur de connexion à la base de données à 14h32" |
| **Métriques** | Données numériques agrégées dans le temps | CPU moyen à 87% pendant 5 minutes |
| **Traces** | Suivi du parcours d'une requête à travers les services | Une requête HTTP traversant 7 microservices en 1,2s |

### Ce qui rend l'observabilité puissante

Contrairement au monitoring, l'observabilité permet de poser des **questions ouvertes** à votre système sans avoir à les anticiper :

- *"Pourquoi les utilisateurs du service X ont-ils des erreurs uniquement depuis hier soir ?"*
- *"Quel microservice ralentit cette transaction précise ?"*
- *"Quelles sont les caractéristiques communes des utilisateurs qui rencontrent ce bug ?"*

C'est ce qu'on appelle les **unknown unknowns** : les problèmes que vous n'auriez jamais imaginé rencontrer.

> **Reprise de l'analogie :** L'observabilité, c'est comme avoir un mécanicien qui peut ouvrir le capot, brancher un diagnostic complet et identifier la pièce défectueuse — même s'il n'avait jamais vu exactement cette panne auparavant.

---

## Tableau comparatif complet

| Critère | Monitoring | Observabilité |
|---------|------------|---------------|
| **Approche** | Réactive | Exploratoire / Proactive |
| **Question posée** | "Est-ce que ça marche ?" | "Pourquoi ça ne marche pas ?" |
| **Type de problèmes** | Known unknowns (problèmes connus) | Unknown unknowns (problèmes inconnus) |
| **Données utilisées** | Métriques prédéfinies uniquement | Logs + Métriques + Traces corrélées |
| **Configuration** | À définir à l'avance (dashboards, alertes) | Investigation à la demande (requêtes ad hoc) |
| **Rôle** | Détecter les symptômes | Diagnostiquer les causes racines |
| **Impact business** | Réduire le MTTD *(Mean Time To Detect)* | Réduire le MTTR *(Mean Time To Resolution)* |

---

## Pourquoi les deux sont complémentaires

Il ne s'agit pas d'opposer monitoring et observabilité. En réalité, **une stratégie efficace combine les deux** :

1. **Le monitoring agit comme une alarme incendie.** Il prévient immédiatement quand quelque chose cloche, déclenchant le processus d'intervention.
2. **L'observabilité agit comme l'enquêteur.** Une fois l'alerte reçue, elle fournit les outils nécessaires pour creuser, corréler les données et trouver la cause exacte.

Sans monitoring, vous apprenez les incidents par vos utilisateurs mécontents. Sans observabilité, vous êtes alerté mais incapable de diagnostiquer rapidement, ce qui allonge considérablement le temps de résolution.

---

## Mise en œuvre concrète

### Pour le monitoring
- Outils : Prometheus, Grafana, Datadog, Zabbix, Nagios
- Bonnes pratiques : Définir des SLO/SLI clairs, éviter le bruit d'alertes, prioriser les alertes actionnables

### Pour l'observabilité
- Outils : OpenTelemetry (standard ouvert), Jaeger ou Tempo (traces), Loki ou ELK (logs), Elasticsearch
- Bonnes pratiques : Instrumenter les applications dès le départ, standardiser le format des logs, assurer la corrélation entre logs/métriques/traces via des identifiants communs (trace ID)

---

## Conclusion

La différence fondamentale tient en une phrase :

> **Le monitoring vous dit qu'il y a un problème ; l'observabilité vous permet de comprendre pourquoi il existe.**

Les architectures modernes — microservices, cloud natif, Kubernetes — ont rendu les systèmes trop complexes pour se reposer uniquement sur le monitoring. Investir dans l'observabilité n'est plus un luxe mais une nécessité pour maintenir la fiabilité et réduire le temps de résolution des incidents.
