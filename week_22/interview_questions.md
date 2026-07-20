# Week 22 — Interview Questions: LLMOps & AIOps — Gateways, Observability & Monitoring

> Sourced from senior LLMOps / platform screens. Where seniors separate from builders.

---

## LLMOps Lifecycle
1. How does LLMOps differ from classic MLOps (non-determinism, prompts, evals, cost)?
2. What does production readiness / governance mean for an LLM app?
3. RAG vs fine-tuning at the ops level — how do you decide?

## Gateways & Routing
4. **Design an LLM gateway that handles provider outages gracefully.**
5. What does LiteLLM give you? How does multi-provider fallback work?
6. Prompt caching (prefix-based) and semantic caching — how do they differ?
7. Rate limiting, retries with backoff, idempotency keys, token budgets.

## Observability & Tracing
8. Why aren't traditional logs enough for agentic apps?
9. **How do you trace a failure across a multi-step agent pipeline?**
10. LangSmith vs Langfuse vs Phoenix vs Braintrust — how do you choose?
11. What are OpenTelemetry GenAI semantic conventions?

## Evaluation in Production
12. What do Ragas / TruLens evaluate? What are the "three gulfs"?
13. What is trace-by-hand + failure bucketing on production traces?
14. What is an online eval and a regression gate?

## AIOps
15. Prometheus + Grafana vs ELK/OpenSearch vs Jaeger — what is each for?
16. Incident management: PagerDuty/Opsgenie — how do you set up alerting?
17. **How do you detect and control a runaway cost incident?**

## Cost & Latency
18. Break down per-query cost (embedding, retrieval, generation).
19. Prefill vs decode phases; how does batching change latency?

## Judgment
20. **What would you monitor for an LLM app that you would NOT monitor for a normal API?**
21. "Self-hosted (Langfuse + ELK) or managed (LangSmith + Datadog) observability?" — defend it.
