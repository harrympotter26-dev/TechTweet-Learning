# 📺 Master Video Search Guide (Weeks 1 to 12)

Copy-paste these exact search terms into **YouTube** (or Google) to get high-quality tutorials without broken links.

---

## Top Recommended Channels for This Roadmap
* **.NET & Architecture:** *Milan Jovanović*, *Nick Chapsas*, *Code Maze*
* **Databases & Protocols:** *Hussein Nasser*, *Brent Ozar*
* **System Design & Scale:** *ByteByteGo*, *Gaurav Sen*
* **DevOps & Cloud:** *TechWorld with Nana*
* **Angular:** *Decoded Frontend*, *Academind*

---

# Week 1: Clean Code, SOLID & Modern Auth

### Monday
* **Morning (8–10 AM): SOLID Principles in C#**
  * **Search:** `Milan Jovanovic SOLID principles C#` OR `Code Maze SOLID C# tutorial`
  * **Duration:** ~45 min
  * **Focus:** Real-world examples of SRP and Dependency Inversion in Web APIs.
* **Evening (9–10 PM): Cryptographic Password Hashing**
  * **Search:** `Computerphile password hashing PBKDF2` OR `Nick Chapsas password hashing C#`
  * **Duration:** ~25 min
  * **Focus:** Salting, pepper, iteration counts, and why MD5/SHA256 alone fail.

### Tuesday
* **Morning (8–10 AM): Stateless Authentication with JWT**
  * **Search:** `Milan Jovanovic JWT authentication ASP.NET Core` OR `Hussein Nasser JWT explained`
  * **Duration:** ~45 min
  * **Focus:** Claims, token generation, cryptographic signing, and validation lifecycle.
* **Evening (9–10 PM): Token Invalidation & Blacklists**
  * **Search:** `JWT revocation strategies Redis blacklist` OR `Nick Chapsas refresh tokens ASP.NET Core`
  * **Duration:** ~25 min
  * **Focus:** Refresh token rotation, Redis blacklisting for immediate logouts.

### Wednesday
* **Morning (8–10 AM): Relational Database Indexing Mechanics**
  * **Search:** `Hussein Nasser database indexing explained` OR `Brent Ozar clustered nonclustered index`
  * **Duration:** ~45 min
  * **Focus:** B-Trees, page lookups, clustered vs. non-clustered index costs.
* **Evening (9–10 PM): Execution Plans & Performance**
  * **Search:** `Brent Ozar read execution plan SQL Server`
  * **Duration:** ~30 min
  * **Focus:** Index seeks vs. table scans, missing index recommendations.

### Thursday
* **Morning (8–10 AM): OAuth 2.0 & OpenID Connect Architecture**
  * **Search:** `TechWorld with Nana OAuth 2 explained` OR `ByteByteGo OAuth 2.0 architecture`
  * **Duration:** ~40 min
  * **Focus:** Grant types, auth code flows, tokens, and redirect callback loops.
* **Evening (9–10 PM): Multi-Factor Authentication (TOTP)**
  * **Search:** `Hussein Nasser TOTP how it works` OR `Code Maze two factor authentication ASP.NET Core`
  * **Duration:** ~25 min
  * **Focus:** RFC 6238 shared secrets, counter windows, and Authenticator app sync.

### Friday
* **Morning (8–10 AM): OWASP Top 10 for .NET Core**
  * **Search:** `Nick Chapsas OWASP security ASP.NET Core`
  * **Duration:** ~45 min
  * **Focus:** SQL injection prevention, parameterized queries, and CORS misconfigurations.
* **Evening (9–10 PM): Writing Technical Engineering Blogs**
  * **Search:** `How to write engineering blog posts software development`
  * **Duration:** ~20 min
  * **Focus:** Structuring system problems, benchmark trade-offs, and architecture diagrams.

---

# Week 2: High-Volume Writes, Caching & Concurrency

### Monday
* **Morning (8–10 AM): Message Queue Decoupling**
  * **Search:** `Hussein Nasser message queues explained` OR `ByteByteGo message queue architecture`
  * **Duration:** ~40 min
  * **Focus:** Producer-consumer patterns, write buffers, and async decoupling.
* **Evening (9–10 PM): Poison Messages & Dead Letter Queues**
  * **Search:** `Dead letter queue explained RabbitMQ Azure Service Bus`
  * **Duration:** ~25 min
  * **Focus:** Failure handling, retry exponential backoff, and DLQ inspection.

### Tuesday
* **Morning (8–10 AM): ACID Transactions & Isolation Levels**
  * **Search:** `Hussein Nasser database isolation levels read committed serializable`
  * **Duration:** ~45 min
  * **Focus:** Dirty reads, phantom reads, and locking strategies in SQL Server.
* **Evening (9–10 PM): Optimistic vs. Pessimistic Concurrency in EF Core**
  * **Search:** `Milan Jovanovic concurrency EF Core` OR `RowVersion concurrency token ASP.NET Core`
  * **Duration:** ~30 min
  * **Focus:** RowVersion timestamps, DB concurrency exceptions, and client-wins resolution.

### Wednesday
* **Morning (8–10 AM): Feed Generation: Push vs. Pull Architectures**
  * **Search:** `ByteByteGo news feed system design` OR `Gaurav Sen Twitter timeline architecture`
  * **Duration:** ~45 min
  * **Focus:** Fanout-on-write vs. fanout-on-read trade-offs for high-profile users.
* **Evening (9–10 PM): Database Denormalization at Scale**
  * **Search:** `Hussein Nasser database normalization vs denormalization`
  * **Duration:** ~30 min
  * **Focus:** Balancing storage redundancy against query joins in high-read systems.

### Thursday
* **Morning (8–10 AM): Distributed Caching Patterns with Redis**
  * **Search:** `ByteByteGo caching strategies` OR `Milan Jovanovic Redis caching ASP.NET Core`
  * **Duration:** ~45 min
  * **Focus:** Cache-aside pattern, read-through, write-through, and data eviction policies.
* **Evening (9–10 PM): Mitigating Cache Stampede & Thundering Herd**
  * **Search:** `Cache stampede mitigation distributed lock` OR `Nick Chapsas hybrid cache .NET 9`
  * **Duration:** ~30 min
  * **Focus:** Mutex locking on cache miss, early refresh windows, and probabilistic expiration.

### Friday
* **Morning (8–10 AM): Soft Deletes & Cascade Invalidation**
  * **Search:** `Milan Jovanovic soft delete EF Core` OR `Redis cache invalidation strategies`
  * **Duration:** ~40 min
  * **Focus:** Query filters in EF Core and event-driven cache invalidation patterns.
* **Evening (9–10 PM): Performance Benchmarking with BenchmarkDotNet**
  * **Search:** `Nick Chapsas BenchmarkDotNet tutorial`
  * **Duration:** ~30 min
  * **Focus:** Memory allocations, execution runtimes, and baseline comparisons.

---

# Week 3: Real-Time Protocols & Scaling WebSockets

### Monday
* **Morning (8–10 AM): WebSockets Architecture & Handshakes**
  * **Search:** `Hussein Nasser WebSockets explained deep dive`
  * **Duration:** ~45 min
  * **Focus:** HTTP upgrade handshake, persistent full-duplex TCP framing.
* **Evening (9–10 PM): Real-Time Hubs with SignalR**
  * **Search:** `Milan Jovanovic SignalR complete guide ASP.NET Core`
  * **Duration:** ~30 min
  * **Focus:** Hubs, transport negotiation (WebSockets, SSE, Long Polling), and clients.

### Tuesday
* **Morning (8–10 AM): Scaling WebSockets Horizontally (Backplanes)**
  * **Search:** `Hussein Nasser scaling WebSockets Redis backplane` OR `SignalR Redis backplane scale out`
  * **Duration:** ~45 min
  * **Focus:** Multi-server broadcast problems, message distribution via Redis Pub/Sub.
* **Evening (9–10 PM): Connection State & Heartbeat Diagnostics**
  * **Search:** `WebSocket keep alive ping pong timeout handling`
  * **Duration:** ~25 min
  * **Focus:** Dropped TCP detection, client reconnection loops, and zombie connections.

### Wednesday
* **Morning (8–10 AM): User Presence Tracking at Scale**
  * **Search:** `System design online offline status presence service ByteByteGo`
  * **Duration:** ~40 min
  * **Focus:** Redis bitmaps, heartbeats, and handling frequent reconnects.
* **Evening (9–10 PM): Ephemeral State Stores in Redis**
  * **Search:** `Redis data structures explained Hussein Nasser`
  * **Duration:** ~30 min
  * **Focus:** Redis Sets, Sorted Sets (ZSET), and TTL-based key expiration.

### Thursday
* **Morning (8–10 AM): Server-Sent Events (SSE) vs. WebSockets**
  * **Search:** `Hussein Nasser WebSockets vs Server Sent Events`
  * **Duration:** ~35 min
  * **Focus:** Unidirectional streaming, HTTP/2 multiplexing advantages, and auto-reconnects.
* **Evening (9–10 PM): Choosing Real-Time Transports**
  * **Search:** `ByteByteGo polling vs long polling vs websockets vs sse`
  * **Duration:** ~25 min
  * **Focus:** Architectural evaluation criteria based on bandwidth, latency, and load.

### Friday
* **Morning (8–10 AM): Load Testing Real-Time Pipelines with k6**
  * **Search:** `k6 load testing tutorial websockets` OR `k6 performance testing API`
  * **Duration:** ~45 min
  * **Focus:** Virtual users, concurrent connection stress tests, and p95/p99 metrics.
* **Evening (9–10 PM): Architecting Real-Time Push Architectures**
  * **Search:** `Event driven push notifications architecture system design`
  * **Duration:** ~30 min
  * **Focus:** Decoupling background workers from push pipelines using internal brokers.

---

# Week 4: API Gateways, Resiliency & Observability

### Monday
* **Morning (8–10 AM): Reverse Proxies & Gateway Patterns**
  * **Search:** `Hussein Nasser reverse proxy vs forward proxy vs API gateway` OR `ByteByteGo API gateway`
  * **Duration:** ~45 min
  * **Focus:** Routing abstractions, SSL termination, and cross-cutting authentication.
* **Evening (9–10 PM): Microsoft YARP (Yet Another Reverse Proxy)**
  * **Search:** `Nick Chapsas YARP tutorial` OR `Milan Jovanovic YARP reverse proxy`
  * **Duration:** ~30 min
  * **Focus:** Route transforms, clusters, reverse proxy pipeline integration in .NET.

### Tuesday
* **Morning (8–10 AM): Rate Limiting Algorithms**
  * **Search:** `ByteByteGo rate limiting algorithms token bucket leaky bucket`
  * **Duration:** ~40 min
  * **Focus:** Token Bucket, Leaky Bucket, Sliding Window Log, and memory limits.
* **Evening (9–10 PM): Built-in Rate Limiting in .NET Core**
  * **Search:** `Milan Jovanovic rate limiting ASP.NET Core .NET 7 .NET 8`
  * **Duration:** ~30 min
  * **Focus:** Partitioned rate limiters, fixed window, concurrency limiters, and handling 429 status codes.

### Wednesday
* **Morning (8–10 AM): Fault Tolerance & Circuit Breakers**
  * **Search:** `ByteByteGo circuit breaker pattern` OR `Hussein Nasser circuit breaker pattern`
  * **Duration:** ~40 min
  * **Focus:** Closed, Open, and Half-Open states to prevent cascading failures.
* **Evening (9–10 PM): Building Resilient Pipelines with Polly**
  * **Search:** `Nick Chapsas Polly resilience .NET` OR `Milan Jovanovic Polly circuit breaker retry`
  * **Duration:** ~35 min
  * **Focus:** Jittered backoffs, fallback strategies, and handling network timeouts.

### Thursday
* **Morning (8–10 AM): Distributed Tracing & W3C Trace Context**
  * **Search:** `OpenTelemetry distributed tracing explained` OR `Hussein Nasser distributed tracing`
  * **Duration:** ~45 min
  * **Focus:** Trace IDs, Span IDs, baggage, and context propagation across process boundaries.
* **Evening (9–10 PM): Structured Logging with Serilog**
  * **Search:** `Milan Jovanovic Serilog structured logging ASP.NET Core`
  * **Duration:** ~30 min
  * **Focus:** JSON payloads, message templates, enriched log properties, and sinks.

### Friday
* **Morning (8–10 AM): Health Checks Architecture in .NET**
  * **Search:** `Nick Chapsas health checks ASP.NET Core`
  * **Duration:** ~35 min
  * **Focus:** Liveness vs. Readiness probes, database check timeouts, and UI reporting.
* **Evening (9–10 PM): Gateway Architecture Documentation**
  * **Search:** `Software architecture C4 model diagrams Simon Brown`
  * **Duration:** ~30 min
  * **Focus:** Diagramming Context, Containers, and Components for production systems.

---

# Week 5: Microservices & Domain-Driven Design (DDD)

### Monday
* **Morning (8–10 AM): Monolith to Microservices Decomposition**
  * **Search:** `ByteByteGo monolithic vs microservices architecture` OR `Martin Fowler microservices`
  * **Duration:** ~45 min
  * **Focus:** Identifying seams, database-per-service anti-patterns, and operational costs.
* **Evening (9–10 PM): Strategic Domain-Driven Design**
  * **Search:** `Milan Jovanovic Domain Driven Design tactical strategic` OR `Bounded contexts DDD explained`
  * **Duration:** ~35 min
  * **Focus:** Bounded contexts, ubiquitous language, and context mapping.

### Tuesday
* **Morning (8–10 AM): Tactical DDD: Entities, Value Objects & Aggregates**
  * **Search:** `Milan Jovanovic Domain Driven Design entities value objects aggregates`
  * **Duration:** ~45 min
  * **Focus:** Enforcing invariants, rich domain models vs. anemic models.
* **Evening (9–10 PM): Domain Events Pattern**
  * **Search:** `Milan Jovanovic Domain Events Clean Architecture`
  * **Duration:** ~30 min
  * **Focus:** Decoupling side effects within an aggregate lifecycle via in-memory events.

### Wednesday
* **Morning (8–10 AM): CQRS (Command Query Responsibility Segregation)**
  * **Search:** `Milan Jovanovic CQRS MediatR architecture` OR `ByteByteGo CQRS explained`
  * **Duration:** ~45 min
  * **Focus:** Splitting read models from write models, query optimization strategies.
* **Evening (9–10 PM): Clean Architecture Directory Structure**
  * **Search:** `Milan Jovanovic Clean Architecture from scratch`
  * **Duration:** ~35 min
  * **Focus:** Domain, Application, Infrastructure, and Presentation dependency directions.

### Thursday
* **Morning (8–10 AM): Outbox Pattern for Reliable Messaging**
  * **Search:** `Milan Jovanovic Outbox pattern microservices` OR `Transactional outbox pattern ByteByteGo`
  * **Duration:** ~45 min
  * **Focus:** Dual-write problems, transactional table persistence, and background publishers.
* **Evening (9–10 PM): Event Publishing with MassTransit**
  * **Search:** `Code Maze MassTransit RabbitMQ tutorial` OR `Milan Jovanovic MassTransit event driven`
  * **Duration:** ~35 min
  * **Focus:** Bus configurations, consumers, message serialization, and exchange topologies.

### Friday
* **Morning (8–10 AM): Saga Pattern for Distributed Transactions**
  * **Search:** `ByteByteGo Saga pattern distributed transactions` OR `Milan Jovanovic Saga orchestration choreography`
  * **Duration:** ~45 min
  * **Focus:** Choreography vs. Orchestration, compensating transactions on failure.
* **Evening (9–10 PM): Database-per-Service Boundary Challenges**
  * **Search:** `Hussein Nasser distributed queries microservices database per service`
  * **Duration:** ~30 min
  * **Focus:** Joins across boundaries, event streaming for projection caching.

---

# Week 6: Containers, Registries & Orchestration

### Monday
* **Morning (8–10 AM): Docker Architecture Internals**
  * **Search:** `TechWorld with Nana Docker tutorial for beginners`
  * **Duration:** ~45 min
  * **Focus:** Namespaces, cgroups, layered file systems, and image creation.
* **Evening (9–10 PM): Optimizing .NET Dockerfiles**
  * **Search:** `Nick Chapsas Dockerize .NET API production best practices`
  * **Duration:** ~30 min
  * **Focus:** Multi-stage builds, rootless user security, and layer caching.

### Tuesday
* **Morning (8–10 AM): Multi-Container Environments with Docker Compose**
  * **Search:** `TechWorld with Nana Docker Compose tutorial`
  * **Duration:** ~40 min
  * **Focus:** Bridge networks, environment passing, volume mounts, and dependency ordering.
* **Evening (9–10 PM): Running Databases and Brokers in Containers**
  * **Search:** `Run SQL Server and RabbitMQ Docker Compose tutorial`
  * **Duration:** ~30 min
  * **Focus:** Persistent volume state, exposed host ports, and initialization scripts.

### Wednesday
* **Morning (8–10 AM): Kubernetes Core Architecture**
  * **Search:** `TechWorld with Nana Kubernetes tutorial for beginners architecture`
  * **Duration:** ~50 min
  * **Focus:** Control plane, Worker nodes, Pods, Deployments, and Kubelet operations.
* **Evening (9–10 PM): Kubernetes Networking & Service Types**
  * **Search:** `TechWorld with Nana Kubernetes Services ClusterIP NodePort LoadBalancer`
  * **Duration:** ~35 min
  * **Focus:** Internal service abstraction, routing between pods, and kube-proxy.

### Thursday
* **Morning (8–10 AM): Kubernetes ConfigMaps & Secrets Management**
  * **Search:** `TechWorld with Nana Kubernetes ConfigMap and Secret`
  * **Duration:** ~35 min
  * **Focus:** Decoupling environment configs and mounting secrets as volumes or environment variables.
* **Evening (9–10 PM): Resource Limits & OOMKill Diagnostics**
  * **Search:** `Kubernetes resource requests limits CPU memory OOMKilled`
  * **Duration:** ~30 min
  * **Focus:** Throttling mechanics, memory footprint management, and autoscaling thresholds.

### Friday
* **Morning (8–10 AM): Ingress Controllers & Routing**
  * **Search:** `TechWorld with Nana Kubernetes Ingress explained`
  * **Duration:** ~45 min
  * **Focus:** Path-based routing, TLS termination, and reverse proxying into cluster services.
* **Evening (9–10 PM): Container Vulnerability Scanning**
  * **Search:** `Docker container security scanning Trivy tutorial`
  * **Duration:** ~25 min
  * **Focus:** Base image vulnerabilities, CVE patching, and container hardening.

---

# Week 7: Azure Core Platform Deployments

### Monday
* **Morning (8–10 AM): Azure Core Cloud Architecture**
  * **Search:** `John Savill Azure architecture fundamentals` OR `TechWorld with Nana Azure tutorial`
  * **Duration:** ~45 min
  * **Focus:** Resource Groups, Subscriptions, Regions, and Virtual Networks.
* **Evening (9–10 PM): Azure CLI Mastery**
  * **Search:** `Azure CLI tutorial for beginners command line deployment`
  * **Duration:** ~30 min
  * **Focus:** Authenticating, creating resource groups, and deploying app resources via command line.

### Tuesday
* **Morning (8–10 AM): Azure App Service Deep Dive**
  * **Search:** `John Savill Azure App Service deep dive` OR `Deploy .NET Core to Azure App Service`
  * **Duration:** ~45 min
  * **Focus:** App Service Plans, scaling tiers, environment variables, and deployment slots.
* **Evening (9–10 PM): Deployment Slots & Zero-Downtime Swaps**
  * **Search:** `Azure App Service deployment slots blue green deployment`
  * **Duration:** ~30 min
  * **Focus:** Warm-up routing, staging environment verification, and instant rollback swaps.

### Wednesday
* **Morning (8–10 AM): Managed Databases with Azure SQL**
  * **Search:** `John Savill Azure SQL Database deep dive`
  * **Duration:** ~45 min
  * **Focus:** DTUs vs. vCore models, geo-redundancy, serverless tiers, and automated backups.
* **Evening (9–10 PM): Securing Azure SQL with Firewall & Private Endpoints**
  * **Search:** `Azure SQL networking private endpoints firewall rules`
  * **Duration:** ~30 min
  * **Focus:** Blocking public IP ingress, VNet service endpoints, and identity authentication.

### Thursday
* **Morning (8–10 AM): Cloud Messaging with Azure Service Bus**
  * **Search:** `John Savill Azure Service Bus deep dive`
  * **Duration:** ~45 min
  * **Focus:** Queues vs. Topics, duplicate detection, dead-lettering, and sessions.
* **Evening (9–10 PM): Integrating Azure Service Bus in .NET Core**
  * **Search:** `Azure.Messaging.ServiceBus .NET tutorial C#`
  * **Duration:** ~35 min
  * **Focus:** Connection multiplexing, batching, and async message handler loops.

### Friday
* **Morning (8–10 AM): Centralized Configuration: Azure Key Vault**
  * **Search:** `John Savill Azure Key Vault deep dive` OR `Azure Key Vault ASP.NET Core integration`
  * **Duration:** ~40 min
  * **Focus:** Secrets, keys, certificates, and runtime injection without local passwords.
* **Evening (9–10 PM): Managed Identity for Passwordless Connections**
  * **Search:** `John Savill Managed Identities deep dive` OR `Azure Managed Identity ASP.NET Core SQL`
  * **Duration:** ~35 min
  * **Focus:** System-assigned vs. User-assigned identities, removing connection strings from config files.

---

# Week 8: CI/CD, Production Observability & Security

### Monday
* **Morning (8–10 AM): Modern CI/CD with GitHub Actions**
  * **Search:** `TechWorld with Nana GitHub Actions tutorial` OR `GitHub Actions for .NET developers`
  * **Duration:** ~45 min
  * **Focus:** Triggers, runners, jobs, steps, environment secrets, and artifact caching.
* **Evening (9–10 PM): Build & Test Pipelines for .NET Solutions**
  * **Search:** `GitHub Actions build and test .NET pipeline tutorial`
  * **Duration:** ~30 min
  * **Focus:** `dotnet restore`, `dotnet build`, running xUnit tests, and fail-on-error configurations.

### Tuesday
* **Morning (8–10 AM): Automated Container Deployment Pipelines**
  * **Search:** `Deploy Docker container to Azure App Service GitHub Actions`
  * **Duration:** ~45 min
  * **Focus:** Publishing to Azure Container Registry (ACR) and triggering webhook redeployments.
* **Evening (9–10 PM): Infrastructure as Code (IaC) with Bicep**
  * **Search:** `John Savill Azure Bicep tutorial for beginners`
  * **Duration:** ~35 min
  * **Focus:** Declarative resource creation, parameter files, and modularizing infrastructure.

### Wednesday
* **Morning (8–10 AM): Application Insights Telemetry**
  * **Search:** `Azure Application Insights tutorial ASP.NET Core`
  * **Duration:** ~45 min
  * **Focus:** Request tracing, dependency mapping, failed requests, and live metrics stream.
* **Evening (9–10 PM): Querying Logs with Kusto (KQL)**
  * **Search:** `KQL tutorial for Application Insights Kusto query language`
  * **Duration:** ~30 min
  * **Focus:** Analyzing exception rates, filtering slow queries, and extracting custom dimensions.

### Thursday
* **Morning (8–10 AM): Auto-Scaling Policies in Production**
  * **Search:** `Azure App Service auto scaling rules CPU metrics`
  * **Duration:** ~35 min
  * **Focus:** Scale-out thresholds, cool-down periods, and cost ceiling protections.
* **Evening (9–10 PM): Azure FinOps: Cloud Cost Optimization**
  * **Search:** `John Savill Azure cost management cost optimization`
  * **Duration:** ~30 min
  * **Focus:** Rightsizing instances, reservations, hybrid benefits, and spending alerts.

### Friday
* **Morning (8–10 AM): End-to-End Production Security Hardening**
  * **Search:** `Azure security best practices identity networking John Savill`
  * **Duration:** ~40 min
  * **Focus:** Defense-in-depth, TLS 1.3 enforcement, and network segmentation.
* **Evening (9–10 PM): Drafting Incident Playbooks & Runbooks**
  * **Search:** `Site Reliability Engineering SRE incident response playbook`
  * **Duration:** ~30 min
  * **Focus:** Outage classification, rollback triggers, and post-mortem templates.

---

# Week 9: Enterprise Angular Architecture

### Monday
* **Morning (8–10 AM): Modern Angular Architecture Essentials**
  * **Search:** `Decoded Frontend modern Angular architecture standalone components`
  * **Duration:** ~45 min
  * **Focus:** Standalone components, dependency injection hierarchies, and module-less project layout.
* **Evening (9–10 PM): Component Lifecycle & Smart/Dumb Component Patterns**
  * **Search:** `Smart vs dumb components Angular pattern`
  * **Duration:** ~30 min
  * **Focus:** Presentational components, container orchestrators, and clean `@Input`/`@Output` boundaries.

### Tuesday
* **Morning (8–10 AM): Reactive Programming with RxJS Operators**
  * **Search:** `Decoded Frontend RxJS operators switchMap mergeMap concatMap exhaustMap`
  * **Duration:** ~45 min
  * **Focus:** Higher-order observables, race condition prevention, and mapping streams.
* **Evening (9–10 PM): Memory Leaks & Subscription Management**
  * **Search:** `Decoded Frontend unsubscribe strategies takeUntilDestroyed async pipe`
  * **Duration:** ~30 min
  * **Focus:** Zombie listeners, `takeUntilDestroyed()`, and managing stream lifecycles.

### Wednesday
* **Morning (8–10 AM): Reactive Forms & Dynamic Validations**
  * **Search:** `Decoded Frontend Angular Reactive Forms validation custom validators`
  * **Duration:** ~45 min
  * **Focus:** `FormGroup`, `FormControl`, async custom validators, and cross-field validation rules.
* **Evening (9–10 PM): Modern Signals Architecture in Angular**
  * **Search:** `Decoded Frontend Angular Signals complete guide`
  * **Duration:** ~35 min
  * **Focus:** Signals, `computed()`, `effect()`, and reactive primitives without RxJS overhead.

### Thursday
* **Morning (8–10 AM): State Management Architecture with NgRx**
  * **Search:** `NgRx store tutorial for beginners actions reducers effects`
  * **Duration:** ~50 min
  * **Focus:** Unidirectional data flow, Store, Actions, Reducers, and Selectors.
* **Evening (9–10 PM): Async Side Effects with NgRx Effects**
  * **Search:** `NgRx Effects tutorial API call handling Decoded Frontend`
  * **Duration:** ~35 min
  * **Focus:** Intercepting actions, dispatching HTTP payloads, and isolating data interactions.

### Friday
* **Morning (8–10 AM): HTTP Interceptors: Authentication & Retries**
  * **Search:** `Angular HTTP interceptor attach JWT refresh token`
  * **Duration:** ~40 min
  * **Focus:** Cloning requests, injecting auth headers, and handling 401 response queues.
* **Evening (9–10 PM): Global Error Handling Services**
  * **Search:** `Angular ErrorHandler service global error handling notification`
  * **Duration:** ~30 min
  * **Focus:** Client-side exception catching, displaying toasts, and logging back to the server.

---

# Week 10: Angular Performance, Security & Testing

### Monday
* **Morning (8–10 AM): Change Detection: OnPush Optimization**
  * **Search:** `Decoded Frontend Angular change detection OnPush explained`
  * **Duration:** ~45 min
  * **Focus:** Immutability, zone.js dirty-checking loops, and triggering `markForCheck()`.
* **Evening (9–10 PM): Angular Profiler & Bundle Analysis**
  * **Search:** `Angular bundle size optimization webpack-bundle-analyzer source-map-explorer`
  * **Duration:** ~30 min
  * **Focus:** Identifying oversized dependencies, tree-shaking failures, and optimizing imports.

### Tuesday
* **Morning (8–10 AM): Code-Splitting with Lazy-Loaded Routes**
  * **Search:** `Angular lazy loading routing standalone components loadComponent`
  * **Duration:** ~40 min
  * **Focus:** `loadChildren`, `loadComponent`, initial load reduction, and preloading strategies.
* **Evening (9–10 PM): Route Guards & Functional Resolvers**
  * **Search:** `Angular functional route guards CanActivateFn resolver`
  * **Duration:** ~30 min
  * **Focus:** Route access gates, token checks, and pre-rendering data loaders.

### Wednesday
* **Morning (8–10 AM): Frontend Security: XSS & CSRF Prevention**
  * **Search:** `Angular security best practices XSS DomSanitizer CSRF`
  * **Duration:** ~40 min
  * **Focus:** Template sanitization, safe URL bypass risks, and HttpOnly cookies.
* **Evening (9–10 PM): Real-Time Client Pipelines with SignalR**
  * **Search:** `Angular SignalR integration real time updates tutorial`
  * **Duration:** ~35 min
  * **Focus:** HubConnection lifecycle, reconnection fallback, and updating UI states on broadcast.

### Thursday
* **Morning (8–10 AM): Component Unit Testing with Jasmine & Karma**
  * **Search:** `Angular unit testing tutorial components services TestBed`
  * **Duration:** ~45 min
  * **Focus:** `TestBed` setups, mock service injection, spy assertions, and trigger events.
* **Evening (9–10 PM): End-to-End Testing with Cypress / Playwright**
  * **Search:** `Playwright Angular E2E testing tutorial for beginners`
  * **Duration:** ~35 min
  * **Focus:** Testing critical paths: login flows, feed interactions, and error assertions.

### Friday
* **Morning (8–10 AM): Production Builds & Cache-Busting**
  * **Search:** `Angular production build deployment optimization hashing`
  * **Duration:** ~35 min
  * **Focus:** Build configurations, asset fingerprinting, gzip/brotli, and NGINX configs.
* **Evening (9–10 PM): Technical Architecture Alignment Review**
  * **Search:** `Full stack architecture review checklist enterprise applications`
  * **Duration:** ~30 min
  * **Focus:** Validating API contracts, cross-origin security, and data consistency.

---

# Week 11: System Design at Enterprise Scale

### Monday
* **Morning (8–10 AM): Horizontal vs. Vertical Scaling & Load Balancing**
  * **Search:** `ByteByteGo load balancing algorithms layer 4 layer 7`
  * **Duration:** ~45 min
  * **Focus:** Round Robin, Least Connections, Consistent Hashing, L4 vs. L7 routing.
* **Evening (9–10 PM): CAP Theorem & Distributed Systems Realities**
  * **Search:** `ByteByteGo CAP theorem explained` OR `Hussein Nasser CAP theorem`
  * **Duration:** ~30 min
  * **Focus:** Consistency, Availability, Partition tolerance trade-offs in distributed data.

### Tuesday
* **Morning (8–10 AM): Database Sharding & Partitioning Strategies**
  * **Search:** `ByteByteGo database sharding` OR `Hussein Nasser database partitioning horizontal vertical`
  * **Duration:** ~45 min
  * **Focus:** Shard keys, rebalancing hotspots, cross-shard joins, and range vs. hash sharding.
* **Evening (9–10 PM): Read Replicas & CQRS Data Synchronization**
  * **Search:** `Hussein Nasser read replicas master slave replication lag`
  * **Duration:** ~35 min
  * **Focus:** Replication lag, read-after-write consistency, and connection routing.

### Wednesday
* **Morning (8–10 AM): Distributed Unique ID Generation**
  * **Search:** `ByteByteGo unique ID generator Twitter snowflake`
  * **Duration:** ~40 min
  * **Focus:** Snowflake IDs, epoch timestamps, worker IDs, and monotonic sequence counters.
* **Evening (9–10 PM): Global Delivery with CDNs and Edge Caching**
  * **Search:** `ByteByteGo CDN content delivery network architecture`
  * **Duration:** ~30 min
  * **Focus:** Edge points of presence, cache invalidation, and static asset delivery.

### Thursday
* **Morning (8–10 AM): Designing Large-Scale Chat Systems**
  * **Search:** `ByteByteGo design a chat system WhatsApp Discord`
  * **Duration:** ~50 min
  * **Focus:** Connection management, message sequencing, persistence, and push delivery.
* **Evening (9–10 PM): Designing Real-Time Notification Pipelines**
  * **Search:** `ByteByteGo notification system design`
  * **Duration:** ~35 min
  * **Focus:** Template deduplication, provider routing, payload security, and rate limiting.

### Friday
* **Morning (8–10 AM): Designing Distributed Storage Systems (YouTube / Netflix)**
  * **Search:** `ByteByteGo design YouTube system design`
  * **Duration:** ~50 min
  * **Focus:** Video chunking, transcoding pipelines, metadata caching, and CDN routing.
* **Evening (9–10 PM): Bottleneck Analysis & Failure Domains**
  * **Search:** `Gaurav Sen system design bottlenecks failure handling`
  * **Duration:** ~30 min
  * **Focus:** Identifying single points of failure, cold-start handling, and load-shedding.

---

# Week 12: Architectural Leadership & Interview Mastery

### Monday
* **Morning (8–10 AM): Documenting Technical Architecture: ADRs**
  * **Search:** `Architecture Decision Records ADR software architecture tutorial`
  * **Duration:** ~40 min
  * **Focus:** Context, decision drivers, considered options, pros/cons, and consequences.
* **Evening (9–10 PM): Conducting Effective Architectural Code Reviews**
  * **Search:** `How to conduct code reviews senior software engineer technical lead`
  * **Duration:** ~30 min
  * **Focus:** Evaluating abstractions, security, scale constraints, and mentoring teams.

### Tuesday
* **Morning (8–10 AM): Portfolio Strategy: Showcasing Architecture on GitHub**
  * **Search:** `Software engineer portfolio GitHub readme architecture projects stand out`
  * **Duration:** ~45 min
  * **Focus:** Professional documentation, architecture diagrams, trade-off matrices, and live metrics.
* **Evening (9–10 PM): Recording System Walkthroughs**
  * **Search:** `How to demo software projects technical presentation tips`
  * **Duration:** ~25 min
  * **Focus:** Clear communication, explaining why decisions were made, and demonstrating system resilience.

### Wednesday
* **Morning (8–10 AM): System Design Interview Frameworks**
  * **Search:** `Gaurav Sen system design interview step by step guide` OR `ByteByteGo system design interview framework`
  * **Duration:** ~50 min
  * **Focus:** Scoping functional/non-functional requirements, back-of-envelope math, and high-level designs.
* **Evening (9–10 PM): Back-of-the-Envelope Capacity Estimations**
  * **Search:** `ByteByteGo back of the envelope estimation system design`
  * **Duration:** ~30 min
  * **Focus:** Storage, QPS, memory caching thresholds, and network bandwidth calculations.

### Thursday
* **Morning (8–10 AM): Mock System Design Interview Session**
  * **Search:** `Exponent system design mock interview senior engineer`
  * **Duration:** ~50 min
  * **Focus:** Pacing, handling ambiguous requirements, trade-off discussions, and lead-level communication.
* **Evening (9–10 PM): Defending Architectural Trade-offs**
  * **Search:** `How to discuss trade offs in system design interviews`
  * **Duration:** ~30 min
  * **Focus:** Explaining why an approach was rejected, cost impacts, and latency trade-offs.

### Friday
* **Morning (8–10 AM): Behavioral Interviews for Lead/Architect Roles**
  * **Search:** `STAR method technical leadership interview questions behavioral`
  * **Duration:** ~45 min
  * **Focus:** Managing technical debt, conflict resolution, production failures, and mentoring.
* **Evening (9–10 PM): Strategic Career Positioning & Compensation Negotiation**
  * **Search:** `Software engineer salary negotiation tech lead senior architect`
  * **Duration:** ~30 min
  * **Focus:** Market alignment, justifying the 20–30 LPA band, and articulating business value.