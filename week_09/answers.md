# Week 9 — Answers with Code: Backend Engineering for AI Products

> APIs, databases, caching, and streaming with FastAPI code.

---

## APIs

### 1. REST idempotency (detailed)
GET/PUT/DELETE are idempotent; POST isn't. Make POST idempotent with a client key.
```python
seen = {}
def create_order(idem_key, payload):
    if idem_key in seen: return seen[idem_key]   # retry-safe
    result = do_create(payload); seen[idem_key] = result
    return result
```

### 2. FastAPI (code)
```python
from fastapi import FastAPI
from pydantic import BaseModel
app = FastAPI()
class Req(BaseModel): prompt: str; max_tokens: int = 256
@app.post("/generate")
async def generate(r: Req):          # auto-validation + docs
    return {"text": await llm(r.prompt, r.max_tokens)}
```

### 3. WebSockets vs SSE (detailed)
SSE: one-way server→client, simple, ideal for token streaming. WebSockets: bidirectional, for interactive chat with live client events.

### 4. Async event loop / blocking (detailed)
Single thread interleaves coroutines at `await`. A blocking call freezes all requests → offload.
```python
from starlette.concurrency import run_in_threadpool
result = await run_in_threadpool(heavy_sync_fn, arg)
```

### 5. API versioning (easy)
Path/header versions (v1, v2), additive changes, deprecate with notice.

---

## Databases

### 6. B-tree indexes (detailed)
Balanced tree → O(log n) lookups + range scans. Hurt on write-heavy tables (every write updates index) and low-cardinality columns.

### 7. EXPLAIN ANALYZE (easy)
Shows chosen plan + real timings/rows → spot seq scans, missing indexes.

### 8. Connection pooling (easy)
Reuse fixed connections (PgBouncer); opening per-request is costly and exhausts the DB.

### 9. Redis caching patterns (detailed + code)
Cache-aside is most common:
```python
def get_user(uid):
    v = redis.get(uid)
    if v: return json.loads(v)          # hit
    v = db.query(uid)                    # miss
    redis.setex(uid, 3600, json.dumps(v))  # TTL to bound staleness
    return v
```

### 10. SQL vs NoSQL vs vector DB (easy)
SQL: relational/transactions; NoSQL: flexible/scale; vector DB: similarity search over embeddings.

---

## LLM Serving

### 11. Stream tokens with SSE (code)
```python
from fastapi.responses import StreamingResponse
@app.post("/stream")
async def stream(r: Req):
    async def gen():
        async for tok in llm_stream(r.prompt):
            yield f"data: {tok}\n\n"     # SSE frame
    return StreamingResponse(gen(), media_type="text/event-stream")
```

### 12. Rate limiting (code)
```python
# token bucket
def allow(bucket, rate, now):
    bucket["tokens"] = min(rate, bucket["tokens"] + (now - bucket["ts"]) * rate)
    bucket["ts"] = now
    if bucket["tokens"] >= 1:
        bucket["tokens"] -= 1; return True
    return False
```

### 13. Queues / backpressure (detailed)
Bounded queue + workers; reject/429 when full so latency stays bounded instead of collapsing.

### 14. Timeouts & retries (easy)
Set per-call timeouts, retry with exponential backoff + jitter, cap attempts, use idempotency keys.

### 15. Circuit breaker (detailed)
After N failures, "open" the circuit and fail fast for a cooldown, then half-open to test recovery — prevents cascading failures.

---

## Coding

### 16. FastAPI streaming endpoint (see Q11).
### 17. Rate limiter (see Q12).
### 18. Idempotent handler (see Q1).

---

## Judgment / Failure

### 19. LLM API slow under load: no continuous batching / connection limits / blocking loop → async, batch, pool, autoscale.
### 20. Cache stampede: many misses hit DB at once on expiry → add jittered TTL, request coalescing/locks, stale-while-revalidate.
### 21. Retries make it worse: retry storm amplifies load → backoff+jitter, circuit breaker, idempotency.
