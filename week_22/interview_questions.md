# Week 22 — Interview Questions: LLMOps & AIOps — Gateways, Observability & Monitoring

> Click a question (▸) to expand the answer.

---

## LLMOps Lifecycle

<details>
<summary>1. How does LLMOps differ from classic MLOps?</summary>

LLMs are non-deterministic, prompt-driven, and API-based; you often don't train the model. Focus shifts to prompt/version management, evals, guardrails, token cost, latency, and tracing rather than training pipelines.
</details>

<details>
<summary>2. Production readiness / governance for an LLM app?</summary>

Versioned prompts/models, eval gates, guardrails, cost controls, observability/tracing, rollback, access control, and compliance/audit — before and after launch.
</details>

<details>
<summary>3. RAG vs fine-tuning at the ops level?</summary>

RAG updates by changing data (easy to keep fresh, more infra). Fine-tuning changes weights (lower latency, but a training/versioning pipeline and staleness). Often combine both.
</details>

## Gateways & Routing

<details>
<summary>4. Design an LLM gateway that handles provider outages gracefully.</summary>

A unified API in front of multiple providers with health checks, automatic fallback/routing, retries with backoff, circuit breakers, semantic + prompt caching, rate limiting, token budgets, and per-request cost/latency logging.
</details>

<details>
<summary>5. What does LiteLLM give you; multi-provider fallback?</summary>

A unified API across providers so you can switch/route models with one interface, and configure fallback so a failed/slow provider automatically reroutes to a backup.
</details>

<details>
<summary>6. Prompt caching (prefix) vs semantic caching?</summary>

Prefix caching reuses computation for a shared prompt prefix (provider-side, exact). Semantic caching returns a stored answer for a semantically similar query (app-side) — huge savings on repeated questions.
</details>

<details>
<summary>7. Rate limiting, retries, idempotency, token budgets?</summary>

Rate limit per user/tenant to protect cost/capacity; retry transient failures with backoff; use idempotency keys so retries don't duplicate; enforce token budgets to cap spend.
</details>

## Observability & Tracing

<details>
<summary>8. Why aren't traditional logs enough for agentic apps?</summary>

One request spans many model/tool/retrieval steps; you need linked traces with inputs/outputs, cost, latency, and decisions per span — flat logs can't show the causal chain.
</details>

<details>
<summary>9. Trace a failure across a multi-step agent pipeline?</summary>

Use a trace id and per-step spans (LangSmith/Langfuse/OpenTelemetry) capturing prompts, tool calls, retrieved docs, and timings — then walk the trace to the failing span.
</details>

<details>
<summary>10. LangSmith vs Langfuse vs Phoenix vs Braintrust?</summary>

All do LLM tracing/evals. LangSmith (LangChain, managed), Langfuse (open-source, self-hostable), Phoenix (open-source, strong on evals/embeddings), Braintrust (eval-focused). Choose by hosting, evals, and stack fit.
</details>

<details>
<summary>11. OpenTelemetry GenAI semantic conventions?</summary>

Standardized span/attribute names for GenAI (model, tokens, prompt, cost) so traces are portable across tools and vendors.
</details>

## Evaluation in Production

<details>
<summary>12. Ragas / TruLens; the "three gulfs"?</summary>

Ragas/TruLens evaluate RAG/agent quality (faithfulness, relevance, groundedness). The three gulfs: comprehension (understand the task), specification (specify what good means), generalization (does it hold on new data).
</details>

<details>
<summary>13. Trace-by-hand + failure bucketing?</summary>

Manually read production traces, label failures into categories (retrieval miss, hallucination, format error), and quantify buckets to prioritize fixes.
</details>

<details>
<summary>14. Online eval and regression gate?</summary>

Online evals score live traffic continuously; a regression gate blocks a deploy if eval metrics drop below a threshold — preventing quality regressions from shipping.
</details>

## AIOps

<details>
<summary>15. Prometheus/Grafana vs ELK vs Jaeger?</summary>

Prometheus + Grafana: metrics + dashboards/alerts. ELK/OpenSearch: log aggregation/search. Jaeger: distributed request tracing. Together they cover metrics, logs, and traces (the three pillars).
</details>

<details>
<summary>16. Incident management (PagerDuty/Opsgenie)?</summary>

They route alerts to on-call, manage escalation, and track incidents so problems get acknowledged and resolved with accountability.
</details>

<details>
<summary>17. Detect and control a runaway cost incident?</summary>

Monitor tokens/cost per request/tenant, alert on anomalies (retry loops, huge contexts, injection), enforce budgets/rate limits, and add circuit breakers to cut off runaway usage automatically.
</details>

## Cost & Latency

<details>
<summary>18. Break down per-query cost?</summary>

Embedding + retrieval + generation (input+output tokens × price), attributed per request/feature/user to find and optimize expensive paths.
</details>

<details>
<summary>19. Prefill vs decode; batching effect?</summary>

Prefill processes the prompt in parallel (compute-heavy); decode generates one token at a time (memory-bandwidth-bound). Batching many requests raises GPU utilization and throughput but can add per-request latency.
</details>

## Judgment

<details>
<summary>20. What would you monitor for an LLM app but NOT a normal API?</summary>

Token usage/cost per request, prompt/model versions, output quality/eval scores, hallucination/guardrail triggers, retrieval relevance, and per-step agent traces — beyond standard latency/error/throughput.
</details>

<details>
<summary>21. "Self-hosted (Langfuse + ELK) or managed (LangSmith + Datadog)?"</summary>

**What I'd do:** self-host (Langfuse + ELK) for data privacy, cost control, and customization; managed (LangSmith + Datadog) for speed and low ops.
**Why:** privacy/cost vs time-to-value.
**Tradeoff:** self-hosting adds operational burden.
**Rejected:** managed when data can't leave your environment; self-hosted when the team is small and wants zero ops.
</details>
