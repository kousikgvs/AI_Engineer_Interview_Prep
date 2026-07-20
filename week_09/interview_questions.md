# Week 9 — Interview Questions: Backend Engineering for AI Products

> Click a question (▸) to expand the answer.

---

## APIs

<details>
<summary>1. REST design; what makes an API idempotent?</summary>

Idempotent = repeating the same request yields the same result/state (GET, PUT, DELETE). POST usually isn't; you make it idempotent with an idempotency key so retries don't duplicate side effects.
</details>

<details>
<summary>2. What does FastAPI give you?</summary>

Async request handling, dependency injection, and automatic request/response validation + docs via Pydantic type hints — fast to build and self-documenting.
</details>

<details>
<summary>3. WebSockets vs SSE for LLM streaming?</summary>

SSE is one-way server→client over HTTP, simple and great for streaming tokens. WebSockets are bidirectional, for interactive/two-way needs. Use SSE for pure token streaming, WebSockets for chat with live client events.
</details>

<details>
<summary>4. Explain the async event loop; what if you block it?</summary>

A single thread interleaves coroutines, switching at await points. A blocking call (heavy CPU, sync I/O) stalls the whole loop, freezing all concurrent requests — offload it to a thread/process pool.
</details>

<details>
<summary>5. How do you version an API safely?</summary>

Version in the path/header (v1, v2), keep old versions running, add fields rather than remove, and deprecate with notice — so existing clients don't break.
</details>

## Databases

<details>
<summary>6. How do B-tree indexes work; when do they hurt?</summary>

A balanced tree enabling O(log n) lookups/range scans. They hurt write-heavy tables (each write updates the index), waste space, and don't help low-cardinality columns.
</details>

<details>
<summary>7. What is EXPLAIN ANALYZE?</summary>

It shows the query planner's chosen plan with actual timings and row counts, revealing seq scans, bad joins, or missing indexes so you can optimize.
</details>

<details>
<summary>8. What is connection pooling (PgBouncer)?</summary>

Reuse a fixed set of DB connections instead of opening one per request. Under load, opening/closing connections is expensive and can exhaust the DB — pooling prevents that.
</details>

<details>
<summary>9. Redis caching patterns?</summary>

Cache-aside (app checks cache, loads DB on miss, writes cache), write-through (write cache+DB together), write-behind (write cache, async to DB). Cache-aside is the common default.
</details>

<details>
<summary>10. Redis vs Postgres for session context?</summary>

Redis for hot, ephemeral, fast-access session state (low latency, TTL eviction); Postgres for durable, queryable, relational data. Often use Redis as a cache in front of Postgres.
</details>

<details>
<summary>11. OLTP vs OLAP?</summary>

OLTP = many small fast transactions (app DB, row-oriented). OLAP = large analytical scans/aggregations (warehouse, column-oriented). Different engines optimized for each.
</details>

## Queues & Background Jobs

<details>
<summary>12. When use a task queue in an AI system?</summary>

For slow/async work — batch embedding, long inference, document processing — so the API responds immediately and workers process jobs in the background.
</details>

<details>
<summary>13. Celery vs ARQ vs RQ vs RabbitMQ vs Kafka?</summary>

Celery/ARQ/RQ are task queues (Python jobs). RabbitMQ is a message broker (routing, low latency). Kafka is a durable, high-throughput event log/stream. Choose by durability, throughput, and ordering needs.
</details>

<details>
<summary>14. Dead-letter queue and retry logic?</summary>

Failed messages retry with backoff; after N failures they move to a dead-letter queue for inspection instead of blocking the pipeline or being lost.
</details>

## Auth & Security

<details>
<summary>15. JWT structure, signing, refresh tokens?</summary>

Header.Payload.Signature, signed so the server can verify without server-side state. Short-lived access tokens + longer refresh tokens balance security and UX.
</details>

<details>
<summary>16. RBAC, API keys, rate limits?</summary>

RBAC grants permissions by role. API keys identify callers. Rate limiting (token bucket/sliding window) protects the service from abuse and runaway cost.
</details>

## Deployment & Distributed Systems

<details>
<summary>17. Docker multi-stage builds and layer caching?</summary>

Multi-stage builds compile in one stage and copy only artifacts into a small runtime image. Ordering Dockerfile steps from least- to most-changing maximizes layer cache reuse and speeds builds.
</details>

<details>
<summary>18. What does Nginx do as reverse proxy/load balancer?</summary>

Terminates SSL, routes/serves requests, balances load across backends, and handles timeouts/buffering — a single robust entry point.
</details>

<details>
<summary>19. CAP theorem; apply to a distributed agent system?</summary>

Under a network partition you can only keep Consistency or Availability, not both. For an agent system, decide per-component: serve possibly-stale state (AP) or refuse until consistent (CP) based on correctness needs.
</details>

<details>
<summary>20. Raft (concept) and eventual consistency?</summary>

Raft elects a leader that replicates a log to followers for consensus/consistency. Eventual consistency means replicas converge over time — reads may be stale but the system stays available.
</details>

## System Design

<details>
<summary>21. Design an API that streams LLM responses to many concurrent users.</summary>

Async FastAPI + SSE/WebSocket streaming, non-blocking model calls (or a queue to inference workers), Redis for caching/session and rate limiting, connection pooling to the DB, autoscaling behind a load balancer, and backpressure/timeouts.
</details>

<details>
<summary>22. Design a chatbot backend with history, caching, rate limiting.</summary>

Store conversation history in Postgres (durable) + Redis (hot window), cache repeated prompts, enforce per-user rate limits and token budgets, stream responses, and run embeddings/summaries as background jobs.
</details>

## Failure / Judgment

<details>
<summary>23. Production API went down under load — fix it.</summary>

Add connection pooling, rate limiting, and move blocking work off the event loop (async or worker queue); add timeouts, health checks, and autoscaling. Root cause: synchronous/blocking calls exhausting the loop and DB connections.
</details>

<details>
<summary>24. "Postgres or Redis for session storage?"</summary>

**What I'd do:** Redis for fast ephemeral session state with TTL; Postgres if sessions must be durable/queryable/audited.
**Why:** Redis gives sub-ms reads and easy expiry.
**Tradeoff:** Redis is in-memory and less durable.
**Rejected:** Postgres-only when latency matters and data is short-lived.
</details>
