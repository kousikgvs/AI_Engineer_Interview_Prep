# Week 18 — Answers with Code: Capstone, System Design & Hiring Sprint

> System design is largely narrative — here are structured answers + key code where it helps.

---

## AI-Native System Design

### 1. Design Perplexity (detailed)
Query understanding → hybrid (web + vector) retrieval → rerank → prompt LLM with sources → stream with inline citations → faithfulness eval. Add caching, rate limiting, cost/latency monitoring, fallback model.
```
Query → [Rewrite/Classify] → [Web Search + Vector DB] → [Rerank] →
[LLM + sources] → [Stream + cite] → [Faithfulness check]
        ↳ cache ↳ rate-limit ↳ metrics ↳ fallback model
```

### 2. Production RAG (detailed)
Ingest → chunk → embed → hybrid index. Query: rewrite/HyDE → hybrid retrieve → cross-encoder rerank → generate with citations → groundedness eval. Add re-indexing, caching, observability.

### 3. Multi-agent coding assistant (detailed)
Coordinator + specialized agents; isolate failures (timeouts, retries, circuit breakers), validate each output, add a critic, human-in-loop for merges, trace every step, bound loops for cost.

### 4. LLM gateway (detailed + code)
Unified API over providers with routing, fallback, caching, rate limiting, budgets, retries, cost tracking.
```python
async def gateway(req):
    if (c := cache.get(req.key)): return c
    for provider in [primary, secondary]:      # automatic fallback
        try:
            r = await provider.call(req, timeout=30)
            cache.set(req.key, r); track_cost(provider, r); return r
        except (Timeout, ProviderError):
            continue
    raise ServiceUnavailable
```

### 5. 10M requests/day (detailed)
CDN/LB → stateless API tier → request queue → autoscaled GPU workers with batching → semantic/response caching → sharded+replicated stores → multi-region, monitoring, graceful degradation.

### 6. ADR (easy)
Architecture Decision Record: context, options, decision, consequences/tradeoffs — so future engineers know why.

---

## Estimation & Trade-offs

### 7. Capacity estimation (code)
```python
# QPS and GPU count
daily = 10_000_000; qps = daily / 86400          # ~116 avg
peak_qps = qps * 3                                # peak factor
per_gpu_qps = 5                                   # measured throughput
gpus = -(-peak_qps // per_gpu_qps)               # ceil
```

### 8. Cost estimation (code)
```python
tokens_per_req = 1500; reqs = 10_000_000
cost_per_1k = 0.002
daily_cost = reqs * tokens_per_req / 1000 * cost_per_1k
```

### 9. Latency budget (detailed)
Split SLO across stages (retrieval 100ms, rerank 50ms, LLM TTFT 300ms, stream). Optimize the critical path; cache the rest.

### 10. Build vs buy (easy)
Buy (API) for speed/quality; self-host for cost-at-scale, privacy, control. Start with API, migrate hot paths.

---

## Reliability

### 11. Graceful degradation (detailed)
On overload/failure: smaller model, cached answer, skip rerank, or queue — degrade quality, not availability.

### 12. Observability (detailed)
Trace each request (retrieval hits, tokens, latency, cost); log prompts/outputs (sampled); dashboards + alerts on quality/cost/latency.

### 13. Canary / rollout (easy)
Ship to small % traffic, watch metrics, auto-rollback on regression.

---

## Interview Practice

### 14. Structure a system design answer (framework)
Requirements → API → data flow → components → scale/bottlenecks → trade-offs → failure modes → monitoring.

### 15. Clarifying questions first (easy)
Scale, latency SLO, quality bar, budget, data sensitivity — before designing.

---

## Judgment / Failure

### 16. Over-engineered design: premature scale → start simple, add complexity when metrics demand.
### 17. No fallback path: single provider/model down → gateway with fallback, cached responses, graceful degradation.
### 18. Costs unmonitored: token blowup → per-request cost tracking, budgets, alerts, caching.
