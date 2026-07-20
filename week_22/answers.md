# Week 22 — Answers with Code: LLMOps & AIOps — Gateways, Observability & Monitoring

> LLM-specific ops: gateways, tracing, cost, caching with code.

---

## LLMOps Lifecycle

### 1. LLMOps vs MLOps (detailed)
LLMs are non-deterministic, prompt-driven, API-based; often no training. Focus: prompt/version management, evals, guardrails, token cost, latency, tracing.

### 2. Production readiness / governance (detailed)
Versioned prompts/models, eval gates, guardrails, cost controls, observability/tracing, rollback, access control, compliance/audit.

### 3. RAG vs fine-tuning at ops level (detailed)
RAG: update by changing data (fresh, more infra). Fine-tuning: change weights (low latency, needs training/versioning, staleness). Often combine.

---

## Gateways & Routing

### 4. LLM gateway for provider outages (detailed + code)
```python
async def route(req, providers):
    for p in providers:                    # ordered by preference/health
        if not p.healthy: continue
        try:
            return await p.call(req, timeout=30)
        except (Timeout, RateLimited, ProviderError):
            p.record_failure()             # trips circuit breaker
            continue
    raise AllProvidersDown
```

### 5. LiteLLM (easy)
Unified API across providers with configurable fallback so a failed/slow provider reroutes to a backup.

### 6. Semantic caching (detailed + code)
Cache by embedding similarity, not exact string → hit on paraphrases.
```python
def semantic_cache_get(query, cache, thresh=0.95):
    q = embed(query)
    hit = cache.nearest(q)
    if hit and cos(q, hit.emb) >= thresh:
        return hit.response
    return None
```

### 7. Model routing (easy)
Route easy queries to a small/cheap model, hard ones to a large model → cost/latency savings.

---

## Observability

### 8. Tracing an LLM request (detailed + code)
Capture prompt, model, tokens, latency, cost, retrieved context per request.
```python
def traced_call(prompt):
    t0 = time.time()
    resp = llm(prompt)
    log_trace(prompt=prompt, response=resp,
              in_tok=count(prompt), out_tok=count(resp),
              latency=time.time()-t0, cost=estimate_cost(prompt, resp))
    return resp
```

### 9. What to monitor (detailed)
Quality (eval scores, thumbs), cost (tokens/$), latency (TTFT, p95), reliability (error/timeout rate), safety (guardrail hits), drift (input/output distributions).

### 10. Token cost tracking (code)
```python
def cost(in_tok, out_tok, in_rate, out_rate):
    return in_tok/1000*in_rate + out_tok/1000*out_rate
```

---

## Monitoring & Quality

### 11. Online eval (detailed)
Sample production traffic, run LLM-as-judge/faithfulness checks + collect user feedback → catch quality regressions live.

### 12. Prompt/version management (easy)
Store prompts as versioned artifacts; A/B test; roll back bad versions without redeploying code.

### 13. Guardrails in prod (detailed)
Input (injection/PII) + output (safety/schema/groundedness) checks; block or fallback on violation.

### 14. Feedback loop (easy)
Collect thumbs/edits → build eval/training data → improve prompts/models over time.

### 15. Alerting (easy)
Alert on cost spikes, latency p95, error rate, quality drops, guardrail-hit surges.

---

## Coding

### 16. Gateway with fallback (see Q4).
### 17. Semantic cache (see Q6).
### 18. Request tracing (see Q8).

---

## Judgment / Failure

### 19. Cost silently 10×: no per-request tracking / prompt bloat → cost dashboards, budgets, caching, model routing.
### 20. Quality dropped after prompt change: no versioning/eval gate → versioned prompts, offline+online eval, canary.
### 21. Provider outage took app down: single provider → gateway with fallback, circuit breakers, cached responses.
