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
- "Design an LLM gateway that handles provider outages gracefully"
- "How do you trace a failure across a multi-step agent pipeline?"
- "What would you monitor for an LLM app that you would NOT monitor for a normal API?"
- "How do you detect and control a runaway cost incident?"

---

## Engineering Judgment Question
**"Self-hosted observability (Langfuse + ELK) or managed (LangSmith + Datadog)?"**
Write your answer covering: what you would do, why, what tradeoff you are making, and what alternative you rejected.
