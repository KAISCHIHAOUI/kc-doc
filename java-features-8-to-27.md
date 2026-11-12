# Java Features by Version: Java 8 to Java 27

## Java 8 — March 2014 (LTS)

The most transformative release in Java's history, introducing a functional programming paradigm.

| JEP | Feature |
|-----|---------|
| 225 | Lambda Expressions |
| 166 | Stream API |
| 166 | `java.util.Optional` |
| 225 | `java.time` API |
| 121 | Default & Static Interface Methods |
| 335 | `@FunctionalInterface` |
| — | Nashorn JavaScript Engine |
| — | Repeating Annotations |
| — | Type Annotations |
| — | Removal of PermGen (replaced by Metaspace) |

**Key details:**
- **Lambda Expressions** — Anonymous function syntax (`(a, b) -> a + b`), enabling concise implementations of functional interfaces.
- **Stream API** — `java.util.stream` package for declarative data pipelines: `map`, `filter`, `reduce`, `collect`, parallel streams.
- **Optional** — `java.util.Optional` as a type-safe alternative to `null` for method return values.
- **java.time** — Complete replacement for `Date`/`Calendar`: `LocalDate`, `LocalTime`, `ZonedDateTime`, `Duration`, `Instant`.
- **Default & Static Interface Methods** — Interfaces can now have method bodies, enabling backward-compatible API evolution.
- **Functional Interfaces** — `@FunctionalInterface` annotation; built-in types like `Function<T,R>`, `Predicate<T>`, `Consumer<T>`, `Supplier<T>`.
- **Nashorn** — Embedded JS runtime (later deprecated in 11, removed in 15).
- **Repeating Annotations** — Same annotation can be applied multiple times to one element.
- **Type Annotations** — Annotations on types (generics, casts, etc.), not just declarations.
- **Removal of PermGen** — Metaspace replaces permanent generation for class metadata.

---

## Java 9 — September 2017

| JEP | Feature |
|-----|---------|
| 261, 200, 220, 260 | Module System / JPMS |
| 322 | JShell |
| — | Collection Factory Methods |
| 246 | Private Interface Methods |
| 286 | Process API |
| 259 | Stack Walking API |
| 238 | Multi-Release JARs |
| — | HTTP/2 Client (preview) |
| 158, 220 | Platform & JVM Logging |

**Key details:**
- **Module System / JPMS** — "Project Jigsaw": named modules with explicit `exports`/`requires`, strong encapsulation of internals, modular JARs, `jlink` for custom runtime images.
- **JShell** — Interactive REPL for Java expressions and statements.
- **Collection Factory Methods** — `List.of()`, `Set.of()`, `Map.of()` for immutable collections.
- **Private Interface Methods** — `private` methods in interfaces for shared helper logic.
- **Process API** — `ProcessHandle` to query and manage OS processes.
- **Stack Walking API** — Efficient, non-throwing stack trace inspection.
- **Multi-Release JARs** — JARs can contain class files for different Java versions.
- **HTTP/2 Client (preview)** — Early version of the modern HTTP client.
- **Platform & JVM Logging** — Unified `java.util.logging` for JDK components.

---

## Java 10 — March 2018

| JEP | Feature |
|-----|---------|
| 286 | Local Variable Type Inference (`var`) |
| 322 | Time-Based Versioning |
| 304 | Garbage-Collector Interface |
| 307 | Parallel Full GC for G1 |
| 310 | Application Class-Data Sharing |

**Key details:**
- **Local Variable Type Inference** — `var list = new ArrayList<String>();` — compiler infers the type.
- **Time-Based Versioning** — Switched from major.minor (10.0) to year-based (10, 11, …).
- **Garbage-Collector Interface** — `java.lang.management.GarbageCollectorMXBean` exposes GC-specific metrics.
- **Parallel Full GC for G1** — G1's full GC runs in parallel, reducing stop-the-world time.
- **Application Class-Data Sharing** — CDS archives for application classes, not just JDK.

---

## Java 11 — September 2018 (LTS)

| JEP | Feature |
|-----|---------|
| 321 | HTTP Client API |
| 330 | Single-File Source Launch |
| 358 | String Enhancements |
| 339 | `Files.readString` / `writeString` |
| 333 | ZGC (experimental) |
| 318 | Epsilon GC |
| 181 | Nest-Based Access Control |
| 309 | Dynamic Class-File Constants |
| 320 | Removal of Java EE & CORBA |
| 323 | Local-Variable Syntax for Lambda Parameters |
| 331 | Low-Overhead Heap Profiling |

**Key details:**
- **HTTP Client API** — `java.net.http.HttpClient` supporting HTTP/1.1, HTTP/2, and WebSockets.
- **Single-File Source Launch** — `java Hello.java` runs a program without explicit compilation.
- **String Enhancements** — `isBlank()`, `strip()`, `stripLeading()`, `stripTrailing()`, `lines()`, `repeat()`.
- **Files.readString / writeString** — One-liner file I/O with explicit charset.
- **ZGC (experimental)** — Low-latency concurrent GC, sub-millisecond pauses.
- **Epsilon GC** — No-op GC for allocation-free benchmarks.
- **Nest-Based Access Control** — Replaces synthetic accessors with nestmate access (inner/outer class).
- **Dynamic Class-File Constants** — `CONSTANT_Dynamic` for future constant types.
- **Removal of Java EE & CORBA** — `java.xml.bind`, `java.activation`, `java.corba`, etc. removed from the JDK.
- **Local-Variable Syntax for Lambda Parameters** — `var` in lambda params: `(var x) -> …`.
- **Low-Overhead Heap Profiling** — JFR-based heap profiling.

---

## Java 12 — March 2019

| JEP | Feature |
|-----|---------|
| 357 | Compact Number Formatting |
| 359 | `Collectors.teeing` |
| 321 | HTTP Client (Standard) |

**Key details:**
- **Compact Number Formatting** — `FormatStyle` for `DecimalFormat` with compact notation (e.g., "1.2M").
- **Collectors.teeing** — Combine three collectors into one (e.g., min-max-range).
- **HTTP Client (Standard)** — Promoted from preview to standard.

---

## Java 13 — September 2019

| JEP | Feature |
|-----|---------|
| 354 | Switch Expressions (Preview) |
| 181 | Nest-Based Access Control (Final) |
| 338 | Vector API (Incubator) |
| 355 | Elastic Metaspace |
| 357 | Unix Domain Socket Channels |

**Key details:**
- **Switch Expressions (Preview)** — Arrow syntax, `yield`, pattern-free switch expressions.
- **Nest-Based Access Control (Final)** — Permanent.
- **Vector API (Incubator)** — Early SIMD vector operations.
- **Elastic Metaspace** — Metaspace can shrink when no longer needed.
- **Unix Domain Socket Channels** — `SocketChannel` for Unix domain sockets.

---

## Java 14 — March 2020

| JEP | Feature |
|-----|---------|
| 361 | Switch Expressions (Second Preview) |
| 358 | Helpful NullPointerExceptions |
| 384 | Records (Preview) |
| 306 | Pattern Matching for `instanceof` (Preview) |
| 378 | Text Blocks (Second Preview) |
| 382 | Reimplement Core Reflection with Method Handles |
| 339 | EdDSA Algorithm |

**Key details:**
- **Switch Expressions (Second Preview)** — Refined arrow syntax with `yield`.
- **Helpful NullPointerExceptions** — Detailed NPE messages identifying the null reference.
- **Records (Preview)** — Concise immutable data carriers: `record Point(int x, int y) {}`.
- **Pattern Matching for `instanceof` (Preview)** — `if (obj instanceof String s)` binds `s` directly.
- **Text Blocks (Second Preview)** — Multi-line strings with `"""`.
- **Reimplement Core Reflection** — Performance and security improvements.
- **EdDSA** — Edwards-curve Digital Signature Algorithm.

---

## Java 15 — September 2020

| JEP | Feature |
|-----|---------|
| 378 | Text Blocks (Final) |
| 360 | Sealed Classes (Preview) |
| 377 | ZGC (Production) |
| 388 | Reimplement `DatagramSocket` |
| 382 | Hidden Classes |
| 372 | Removal of Nashorn |

**Key details:**
- **Text Blocks (Final)** — Triple-quoted multi-line strings with implicit concatenation and backslash continuation.
- **Sealed Classes (Preview)** — Restrict which classes can extend/implement: `sealed class Shape permits Circle, Square`.
- **ZGC (Production)** — Low-latency GC promoted to production-ready.
- **Reimplement `DatagramSocket`** — Performance improvements.
- **Hidden Classes** — JVM-internal classes for dynamic code generation.
- **Removal of Nashorn** — JavaScript engine removed from the JDK.

---

## Java 16 — March 2021

| JEP | Feature |
|-----|---------|
| 395 | Records (Final) |
| 394 | Pattern Matching for `instanceof` (Final) |
| 397 | Sealed Classes (Second Preview) |
| 414 | Vector API (Incubator) |
| 416 | Reimplement Core Reflection (Final) |

**Key details:**
- **Records (Final)** — Transparent immutable data carriers with auto-generated `equals`, `hashCode`, `toString`, accessors.
- **Pattern Matching for `instanceof` (Final)** — `if (obj instanceof String s)` — no cast needed.
- **Sealed Classes (Second Preview)** — Refined with `permits` clause.
- **Vector API (Incubator)** — SIMD-style vector operations for numeric types.
- **Reimplement Core Reflection (Final)** — Method Handles under the hood.

---

## Java 17 — September 2021 (LTS)

| JEP | Feature |
|-----|---------|
| 409 | Sealed Classes (Final) |
| 403 | Strongly Encapsulate JDK Internals |
| 406 | Pattern Matching for `switch` (Preview) |
| 356 | Enhanced Pseudo-Random Number Generators |
| 306 | Restore Always-Strict Floating-Point Semantics |
| 382 | New macOS Rendering Pipeline |
| 391 | macOS/AArch64 Port |
| 398 | Deprecate Applet API for Removal |
| 407 | Remove RMI Activation |

**Key details:**
- **Sealed Classes (Final)** — `sealed`, `non-sealed`, `final`, `permits` keywords to constrain subtype hierarchies.
- **Strongly Encapsulate JDK Internals** — All `sun.*` and internal APIs now strongly encapsulated; `--illegal-access` removed.
- **Pattern Matching for `switch` (Preview)** — Type patterns and `case null` in switch.
- **Enhanced Pseudo-Random Number Generators** — `RandomGenerator` interface, `RandomGeneratorFactory`, implementations.
- **Always-Strict Floating-Point** — `strictfp` removed; all floating-point is always strict.
- **New macOS Rendering Pipeline** — Java 2D on macOS uses Metal.
- **macOS/AArch64 Port** — Native Apple Silicon support.
- **Deprecate Applet API for Removal**.
- **Remove RMI Activation**.

---

## Java 18 — March 2022

| JEP | Feature |
|-----|---------|
| 400 | UTF-8 by Default |
| 408 | Simple Web Server |
| 413 | Code Snippets in Java API Documentation |
| 416 | Reimplement Core Reflection with Method Handles (Final) |
| 417 | Vector API (Third Incubator) |
| 418 | Internet-Address Resolution SPI |
| 412 | Foreign Function & Memory API (Fourth Incubator) |
| 420 | Pattern Matching for `switch` (Second Preview) |

**Key details:**
- **UTF-8 by Default** — `file.encoding` and `native.encoding` default to `UTF-8`.
- **Simple Web Server** — `jwebserver` for quick static file serving.
- **Code Snippets in JavaDoc** — `@snippet` tag for reusable doc examples.
- **Reimplement Core Reflection (Final)** — Method Handles under the hood.
- **Vector API (Third Incubator)**.
- **Internet-Address Resolution SPI** — Custom DNS resolution.
- **Foreign Function & Memory API (Fourth Incubator)** — Safe, fast native interop replacing JNI for many use cases.
- **Pattern Matching for `switch` (Second Preview)**.

---

## Java 19 — September 2022

| JEP | Feature |
|-----|---------|
| 426 | Foreign Function & Memory API (Fifth Incubator) |
| 428 | Vector API (Fourth Incubator) |
| 427 | Pattern Matching for `switch` (Third Preview) |
| 429 | Hidden Classes for JVM-Internal Use |
| 428 | Structured Concurrency (Preview) |

**Key details:**
- **Foreign Function & Memory API (Fifth Incubator)** — `SegmentAllocator`, `MemorySegment`, `Linker` for native memory.
- **Vector API (Fourth Incubator)**.
- **Pattern Matching for `switch` (Third Preview)**.
- **Hidden Classes for JVM-Internal Use** — Replaced with `MethodHandles.Lookup.defineHiddenClass`.
- **Structured Concurrency (Preview)** — `StructuredTaskScope` for scoped, cancellable concurrent tasks.

---

## Java 20 — March 2023

| JEP | Feature |
|-----|---------|
| 432 | Record Patterns (Preview) |
| 433 | Pattern Matching for `switch` (Fourth Preview) |
| 436 | Virtual Threads (Third Preview) |
| 431 | Sequenced Collections (Preview) |
| 430 | String Templates (Preview) |
| 446 | Scoped Values (Preview) |
| 437 | Structured Concurrency (Second Preview) |

**Key details:**
- **Record Patterns (Preview)** — Destructure records in patterns: `case Point(int x, int y)`.
- **Pattern Matching for `switch` (Fourth Preview)** — Guard conditions (`when`), irrefinable patterns.
- **Virtual Threads (Third Preview)** — Lightweight, JVM-managed threads (Project Loom).
- **Sequenced Collections (Preview)** — `SequencedCollection` interface: `getFirst()`, `getLast()`, `reversed()`.
- **String Templates (Preview)** — Interpolated strings: `STR"Hello \{name}"`.
- **Scoped Values (Preview)** — Immutable, inheritable per-thread data (alternative to `ThreadLocal`).
- **Structured Concurrency (Second Preview)**.

---

## Java 21 — September 2023 (LTS)

| JEP | Feature |
|-----|---------|
| 444 | Virtual Threads (Final) |
| 440 | Record Patterns (Final) |
| 441 | Pattern Matching for `switch` (Final) |
| 431 | Sequenced Collections (Final) |
| 439 | Generational ZGC |
| 443 | Unnamed Patterns & Variables (Preview) |
| 445 | Unnamed Classes & Instance Main Methods (Preview) |
| 446 | Scoped Values (Preview) |
| 430 | String Templates (Preview) |

**Key details:**
- **Virtual Threads (Final)** — Millions of lightweight threads managed by the JVM; `Thread.ofVirtual()`, `Executors.newVirtualThreadPerTaskExecutor()`.
- **Record Patterns (Final)** — Destructure records in `switch` and `instanceof`.
- **Pattern Matching for `switch` (Final)** — Full pattern matching: type patterns, record patterns, guard conditions, `case null`.
- **Sequenced Collections (Final)** — `SequencedCollection`, `SequencedSet`, `SequencedMap` interfaces.
- **Generational ZGC** — ZGC with young/old generations; better throughput and memory efficiency.
- **Unnamed Patterns & Variables (Preview)** — `_` as a wildcard variable.
- **Unnamed Classes & Instance Main Methods (Preview)** — Prototype-style entry points.
- **Scoped Values (Preview)**.
- **String Templates (Preview)** — (Later withdrawn in 23.)

---

## Java 22 — March 2024

| JEP | Feature |
|-----|---------|
| 456 | Unnamed Variables and Patterns (Final) |
| 454 | Foreign Function & Memory API (Final) |
| 461 | Multi-File Source-Code Programs |
| 466 | Class-File API (First Preview) |
| 463 | Stream Gatherers (First Preview) |
| 459 | String Templates (Second Preview) |
| 447 | Flexible Constructor Bodies (First Preview) |
| 463 | Unnamed Classes and Instance Main Methods (Second Preview) |
| 474 | Region Pinning for G1 |
| 471 | Deprecate `sun.misc.Unsafe` Memory-Access Methods |

**Key details:**
- **Unnamed Variables and Patterns (Final)** — `_` as a throwaway variable/pattern.
- **Foreign Function & Memory API (Final)** — `MemorySegment`, `Linker`, `SegmentAllocator` for safe native interop.
- **Multi-File Source-Code Programs** — `java file1.java file2.java` runs multiple source files together.
- **Class-File API (First Preview)** — Read/write class files programmatically.
- **Stream Gatherers (First Preview)** — Custom stream transformations (e.g., `windowed`, `sliding`).
- **String Templates (Second Preview)**.
- **Flexible Constructor Bodies (First Preview)** — Statements before `super()`/`this()`.
- **Unnamed Classes and Instance Main Methods (Second Preview)**.
- **Region Pinning for G1** — G1 can pin regions to reduce humongous allocation.
- **Deprecate `sun.misc.Unsafe` Memory-Access Methods**.

---

## Java 23 — September 2024

| JEP | Feature |
|-----|---------|
| 467 | Markdown Documentation Comments |
| 474 | ZGC: Generational Mode by Default |
| 473 | Stream Gatherers (Second Preview) |
| 455 | Primitive Types in Patterns, `instanceof`, and `switch` (Preview) |
| 466 | Class-File API (Second Preview) |
| 476 | Module Import Declarations (Preview) |
| 477 | Implicitly Declared Classes and Instance Main Methods (Third Preview) |
| 480 | Structured Concurrency (Third Preview) |
| 481 | Scoped Values (Third Preview) |
| 482 | Flexible Constructor Bodies (Second Preview) |
| — | String Templates — **Withdrawn** |

**Key details:**
- **Markdown Documentation Comments** — Javadoc now supports GitHub-flavored Markdown.
- **ZGC: Generational Mode by Default** — Generational ZGC is now the default ZGC mode.
- **Stream Gatherers (Second Preview)** — Refined `Gatherer` API.
- **Primitive Types in Patterns (Preview)** — Pattern-match on primitives.
- **Class-File API (Second Preview)**.
- **Module Import Declarations (Preview)** — `import module java.base;`
- **Implicitly Declared Classes and Instance Main Methods (Third Preview)**.
- **Structured Concurrency (Third Preview)**.
- **Scoped Values (Third Preview)**.
- **Flexible Constructor Bodies (Second Preview)**.
- **String Templates — Withdrawn** — Removed due to design concerns; a new approach is planned.

---

## Java 24 — March 2025

| JEP | Feature |
|-----|---------|
| 487 | Stream Gatherers (Final) |
| 496 | Class-File API (Final) |
| 450 | Compact Object Headers (Experimental → Product) |
| 483 | Ahead-of-Time Class Loading & Linking |
| 488 | Primitive Types in Patterns (Second Preview) |
| 494 | Module Import Declarations (Second Preview) |
| 495 | Simple Source Files and Instance Main Methods (Fourth Preview) |
| 492 | Flexible Constructor Bodies (Third Preview) |
| 491 | Structured Concurrency (Fourth Preview) |
| 493 | Scoped Values (Fourth Preview) |
| 498 | Vector API (Ninth Incubator) |
| 478 | Key Derivation Function API (Preview) |
| 470 | PEM Encodings of Cryptographic Objects (Preview) |

**Key details:**
- **Stream Gatherers (Final)** — `StreamGatherer` for custom stream pipelines.
- **Class-File API (Final)** — Programmatic class file reading/writing.
- **Compact Object Headers** — 64-bit object headers (down from 96-bit), reducing heap by up to ~10%.
- **Ahead-of-Time Class Loading & Linking** — Pre-link classes at build time for faster startup (Project Leyden).
- **Primitive Types in Patterns (Second Preview)**.
- **Module Import Declarations (Second Preview)**.
- **Simple Source Files and Instance Main Methods (Fourth Preview)**.
- **Flexible Constructor Bodies (Third Preview)**.
- **Structured Concurrency (Fourth Preview)**.
- **Scoped Values (Fourth Preview)**.
- **Vector API (Ninth Incubator)**.
- **Key Derivation Function API (Preview)** — KDF algorithms (HKDF, etc.).
- **PEM Encodings of Cryptographic Objects (Preview)**.

---

## Java 25 — September 2025 (LTS)

The first LTS since 21, with 18 JEPs — seven finalized, nine performance/runtime-focused.

| JEP | Feature | Status |
|-----|---------|--------|
| 512 | Compact Source Files & Instance Main Methods | Final |
| 511 | Module Import Declarations | Final |
| 513 | Flexible Constructor Bodies | Final |
| 506 | Scoped Values | Final |
| 510 | Key Derivation Function API | Final |
| 519 | Compact Object Headers | Final (opt-in flag) |
| 521 | Generational Shenandoah | Final (opt-in) |
| 514 | AOT Command-Line Ergonomics | Final |
| 515 | AOT Method Profiling | Final |
| 518 | JFR Cooperative Sampling | Final |
| 520 | JFR Method Timing & Tracing | Final |
| 507 | Primitive Types in Patterns | Third Preview |
| 502 | Stable Values | Preview |
| 505 | Structured Concurrency | Fifth Preview |
| 508 | Vector API | Tenth Incubator |
| 470 | PEM Encodings | Preview |
| 509 | JFR CPU-Time Profiling | Experimental (Linux) |
| 503 | Remove 32-bit x86 Port | Final |

**Key highlights:**
- **Compact Source Files** — Write a valid Java program with just `void main() { IO.println("Hi"); }` — no class declaration, no `static`, no `String[] args`.
- **Module Imports** — `import module java.base;` imports all exported packages at once.
- **Flexible Constructors** — Validate arguments *before* calling `super()`: `if (x < 0) throw …; super(x);`
- **Scoped Values** — Thread-safe, immutable data propagation to child threads; integrates with virtual threads and structured concurrency.
- **Compact Object Headers** — Each object's header shrinks from 12→8 bytes on 64-bit, saving up to 20% heap for object-heavy apps.
- **AOT Caching** — Training runs record class loading, linking, and method profiles; subsequent starts load from cache, cutting startup time dramatically.

---

## Java 26 — March 2026

10 JEPs; first non-LTS after Java 25.

| JEP | Feature | Status |
|-----|---------|--------|
| 530 | Primitive Types in Patterns, `instanceof`, `switch` | Fourth Preview |
| 516 | Ahead-of-Time Object Caching with Any GC | Final |
| 517 | HTTP/3 for the HTTP Client API | Final |
| 522 | G1 GC: Improve Throughput by Reducing Synchronization | Final |
| 524 | PEM Encodings of Cryptographic Objects | Second Preview |
| 525 | Structured Concurrency | Sixth Preview |
| 526 | Lazy Constants | Second Preview |
| 500 | Prepare to Make Final Mean Final | Final (Warning) |
| 504 | Remove the Applet API | Final |
| — | Vector API | Eleventh Incubator |

**Key highlights:**
- **HTTP/3** — `HttpClient` now supports QUIC/HTTP/3; opt-in, defaults to HTTP/2.
- **AOT Object Caching with Any GC** — Pre-initialized objects cached in a GC-agnostic format; works with ZGC, removing the previous limitation.
- **G1 Throughput** — Reduced synchronization between mutator and GC threads via a dual card table (5–15% gain).
- **Lazy Constants** — `LazyConstant` objects: unmodifiable data the JVM optimizes like `final` fields, but with deferred initialization (useful for AI/data-driven apps).
- **Applet Removal** — The last vestiges of Java Applets are gone.
- **Post-Quantum JAR Signing** — ML-DSA signed JARs supported.
- **Unicode 17.0** — 4,803 new characters, 4 new scripts.

---

## Java 27 — September 2026

9 JEPs; second non-LTS after Java 25.

| JEP | Feature | Status |
|-----|---------|--------|
| 523 | G1 as Default GC Everywhere | Final |
| 534 | Compact Object Headers by Default | Final |
| 527 | Post-Quantum Hybrid Key Exchange for TLS 1.3 | Final |
| 536 | JFR In-Process Data Redaction | Final |
| 531 | Lazy Constants | Third Preview |
| 532 | Primitive Types in Patterns, `instanceof`, `switch` | Fifth Preview |
| 533 | Structured Concurrency | Seventh Preview |
| 538 | PEM Encodings of Cryptographic Objects | Third Preview |
| — | Vector API | Twelfth Incubator |

**Key highlights:**
- **G1 Everywhere** — Previously, the JVM chose Serial GC on single-CPU or <1,792 MB RAM machines. Now G1 is always the default. Performance on constrained environments is comparable or better.
- **Compact Object Headers by Default** — What was opt-in in 24/25 is now the default. Saves up to 20% heap and up to 10% throughput for object-heavy apps. Opt out with `-XX:-UseCompactObjectHeaders`.
- **Post-Quantum TLS** — TLS 1.3 handshakes now use hybrid key exchange: a classical algorithm (e.g., X25519) *combined* with ML-KEM. Defends against "harvest now, decrypt later" attacks. Transparent to existing `javax.net.ssl` code.
- **JFR Data Redaction** — Sensitive values in environment variables, system properties, and program arguments are automatically redacted in JFR event streams, closing a data-leak vector.
- **Performance Gains** — ML-KEM, ML-DSA, X25519, and Ed25519 algorithms significantly faster.

---

## LTS Timeline

| Version | Release | Support Until (Oracle) |
|---------|---------|----------------------|
| Java 8 | Mar 2014 | Extended (commercial) |
| Java 11 | Sep 2018 | Sep 2032 |
| Java 17 | Sep 2021 | Sep 2029 |
| Java 21 | Sep 2023 | Sep 2031 |
| Java 25 | Sep 2025 | Sep 2033 (est.) |
| Java 29 | Sep 2027 (est.) | — |

The next LTS after 25 is expected to be **Java 29** (September 2027), following the two-year LTS cadence.   
