# Agent Guide — System Design & Architecture

> Designing systems that scale, plus the everyday architecture that keeps a codebase sane: folder structure, naming, DB and API design.
> Category: Architecture & Systems · Source: https://aliarsalan177.github.io/guides/system-design/

## Purpose

A field guide for **AI coding agents** (and engineers): the concepts to apply and the
mistakes to avoid when building with System Design & Architecture. Load this into your agent's context so it
keeps these concepts in mind and does **not** repeat these mistakes.

## Rules — Apply / Avoid

### 1. Design from requirements & constraints
- ✅ **APPLY:** Clarify functional + non-functional needs (scale, latency, consistency), estimate load, then choose components.
- ⛔ **AVOID:** Jumping to a microservices/Kafka diagram before you understand the actual requirements and scale.

### 2. Scale with statelessness, caching & queues
- ✅ **APPLY:** Keep app servers stateless (scale horizontally behind a load balancer), cache hot reads, and use queues to smooth spikes.
- ⛔ **AVOID:** Sticky server state, hammering the DB for every read, and doing slow work synchronously in the request path.

### 3. Model data for access patterns
- ✅ **APPLY:** Normalize for integrity, denormalize/index for read performance; pick SQL vs NoSQL by consistency & query needs.
- ⛔ **AVOID:** One-size-fits-all schemas, missing indexes on filtered columns, and N+1 queries.

### 4. Consistent, predictable structure & naming
- ✅ **APPLY:** Organize by feature/domain, colocate related files, and name consistently (PascalCase components, kebab-case files/routes, plural table names).
- ⛔ **AVOID:** Mixing casing conventions, dumping everything in one folder, and vague names (utils.js, data.ts, Manager).

### 5. Design clear API contracts
- ✅ **APPLY:** Use consistent REST resources (or GraphQL schema), proper status codes, versioning, pagination, and validation.
- ⛔ **AVOID:** Chatty, inconsistent endpoints, leaking internal models, and breaking changes with no versioning.

## Cheat Reference — concepts to remember

- **Design from requirements & constraints** — Clarify functional + non-functional needs (scale, latency, consistency), estimate load, then choose components.
- **Scale with statelessness, caching & queues** — Keep app servers stateless (scale horizontally behind a load balancer), cache hot reads, and use queues to smooth spikes.
- **Model data for access patterns** — Normalize for integrity, denormalize/index for read performance; pick SQL vs NoSQL by consistency & query needs.
- **Consistent, predictable structure & naming** — Organize by feature/domain, colocate related files, and name consistently (PascalCase components, kebab-case files/routes, plural table names).
- **Design clear API contracts** — Use consistent REST resources (or GraphQL schema), proper status codes, versioning, pagination, and validation.

## Full Cheat Sheet — every concept

### Scalability Building Blocks
- Load balancer + stateless app servers (horizontal scale); vertical vs horizontal scaling.
- Caching layers: CDN (edge), Redis/Memcached (app), DB query cache; cache invalidation strategy.
- Async: message queues (SQS/Kafka/RabbitMQ), workers, event-driven; rate limiting & backpressure.

### Data & DB Design
- Normalization (1NF–3NF) for integrity; denormalize + index for reads.
- Primary/foreign keys, indexes on filtered/joined columns; avoid N+1.
- Scaling: replication (read replicas), sharding/partitioning; SQL vs NoSQL by consistency & pattern.
- Transactions & ACID vs BASE/eventual consistency; CAP trade-offs.

### Architecture Styles
- Monolith (simple, one deploy) → modular monolith → microservices (independent scale/deploy, more ops).
- Layered / hexagonal / clean architecture; separation of concerns; one-way dependencies.
- Sync (REST/GraphQL/gRPC) vs async (events); API gateway; BFF.

### File / Folder & Naming
- Feature/domain-based folders; colocate component + style + test + types.
- Naming: PascalCase components/classes, camelCase vars/functions, kebab-case files/routes/URLs, plural DB tables, snake_case columns.
- index files for public API of a module; keep imports flowing inward.

### API Design
- REST: resource nouns, proper verbs + status codes, pagination, filtering, versioning (/v1).
- GraphQL: typed schema, resolvers, avoid over/under-fetching; validate all input.
- Idempotency, error format consistency, auth (OAuth2/JWT), rate limits.

### Reliability
- Redundancy & failover, health checks, graceful degradation, circuit breakers, retries with backoff.
- Observability: logs, metrics, traces, alerts; SLAs/SLOs.

## Interview Questions

#### Q1. How do you approach a system design question?
Clarify requirements (functional + non-functional), estimate scale (QPS, data size), define the API and data model, sketch the high-level architecture (LB, app tier, cache, DB, queue), then dig into bottlenecks, trade-offs, and failure modes.

#### Q2. How do you scale a read-heavy system?
Add caching (CDN, Redis), read replicas, and denormalized read models; keep app servers stateless behind a load balancer; use pagination and indexes. Introduce queues/async for writes and heavy work.

#### Q3. SQL vs NoSQL — how do you choose?
SQL for strong consistency, relations, and complex queries/transactions; NoSQL (document/key-value/wide-column) for massive scale, flexible schemas, and simple access patterns. Match the store to the access pattern; polyglot persistence is common.

#### Q4. What's the CAP theorem?
Under a network partition, a distributed system can guarantee at most two of Consistency, Availability, Partition tolerance — since partitions happen, you effectively trade consistency vs availability (CP vs AP).

#### Q5. How should you structure a project's files/folders?
Prefer feature/domain-based grouping (colocate component, styles, tests, logic) over type-based once the app grows, keep a consistent naming convention, and enforce clear module boundaries so dependencies flow one way.

---

_Curated by Ali Arsalan · https://aliarsalan177.github.io · Generated from the Engineering Guides._
