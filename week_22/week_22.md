# Week 22 — LLMOps & AIOps: Gateways, Observability & Monitoring

**Block:** I — MLOps, LLMOps & AIOps (Advanced Depth Track)
**Goal:** Operate LLM and agent systems in production — routing, caching, tracing, monitoring, incident response, and cost control. This is where senior AI engineers separate from builders.

---

## What You Will Learn

### The LLMOps Lifecycle
- LLMOps lifecycle: ideation → development → operations
- How LLMOps differs from classic MLOps (non-determinism, prompts, evals, cost)
- Governance and production readiness for LLM apps
- Choosing between RAG and fine-tuning at the ops level

### LLM Gateways & Routing
- LiteLLM — unified API across providers (OpenAI, Anthropic, Gemini, open models)
- Multi-provider routing with automatic fallback
- Prompt caching (Anthropic/OpenAI) and prefix-based cache design
- Semantic caching for repeated queries
- Rate limiting, retries with exponential backoff, idempotency keys
- Token budgets and per-tenant quotas

### Observability & Tracing
- Why traditional logs are not enough for agentic apps
- LangSmith, Langfuse, Braintrust, Phoenix, OpenLLMetry
- OpenTelemetry GenAI semantic conventions
- Tracing model calls, tool calls, retrieval, and multi-step agent workflows
- What to log: inputs, outputs, latency, cost, model version, retrieved docs

### Evaluation in Production
- Ragas, TruLens — RAG and agent evaluation
- LLM-as-judge with strong judge alignment
- The three gulfs: comprehension, specification, generalization
- Trace-by-hand workflow and failure bucketing on production traces
- Online evals and regression gates

### AIOps — Operating the Infrastructure
- Monitoring: Prometheus + Grafana
- Logging: Elasticsearch, Logstash, Kibana (ELK) / OpenSearch
- Distributed tracing: Jaeger + OpenTelemetry
- Incident management: PagerDuty, Opsgenie
- Automation: Ansible, Terraform, Argo CD
- Anomaly detection and alerting on cost/quality/latency

### Cost & Latency Optimization
- Per-query cost breakdown: embedding, retrieval, generation
- Cost anomaly detection and budget guardrails
- Latency economics: P50/P90/P99 and user experience
- Prompt compression; batching, prefill vs decode phases

---

## Project
- Build a production LLM Gateway with LiteLLM: multi-provider routing, fallback, semantic caching, rate limiting, and a cost dashboard
- Instrument a Week 13/14 agent with Langfuse + OpenTelemetry end-to-end tracing
- Stand up Prometheus + Grafana + ELK for infra observability
- Add Ragas online evals with a regression gate and PagerDuty alerting on quality/cost anomalies

---

## Paper / Reading
- Read: "Your AI Product Needs Evals" (Hamel Husain) + Google SRE Book — "Monitoring Distributed Systems" chapter

---

## Failure Friday
- Analyze: An agent platform where a single provider outage cascaded to full downtime — no gateway, no fallback, no circuit breaker. Design the fix.

---

## Interview Questions This Week Prepares You For

<details>
<summary>"Design an LLM gateway that handles provider outages gracefully"</summary>

A unified API in front of multiple providers with health checks, automatic fallback/routing, retries with backoff, circuit breakers, semantic + prompt caching, rate limiting, token budgets, and cost/latency logging.
</details>

<details>
<summary>"How do you trace a failure across a multi-step agent pipeline?"</summary>

Use a trace id and per-step spans (Langfuse/OpenTelemetry) capturing prompts, tool calls, retrieved docs, and timings, then walk the trace to the failing span.
</details>

<details>
<summary>"What would you monitor for an LLM app that you would NOT for a normal API?"</summary>

Token usage/cost per request, prompt/model versions, output quality/eval scores, hallucination/guardrail triggers, retrieval relevance, and per-step agent traces.
</details>

<details>
<summary>"How do you detect and control a runaway cost incident?"</summary>

Monitor tokens/cost per request/tenant, alert on anomalies (retry loops, huge contexts, injection), enforce budgets/rate limits, and add circuit breakers to cut off runaway usage automatically.
</details>

---

## Engineering Judgment Question

<details>
<summary><strong>"Self-hosted observability (Langfuse + ELK) or managed (LangSmith + Datadog)?"</strong></summary>

**What I'd do:** self-host for data privacy, cost control, and customization; managed for speed and low ops.
**Why:** privacy/cost vs time-to-value.
**Tradeoff:** self-hosting adds operational burden.
**Rejected:** managed when data can't leave your environment; self-hosted when the team wants zero ops.
</details>
