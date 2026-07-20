# Week 10 — Answers with Code: Data Engineering

> Data modeling, DB internals, pipelines. Conceptual + SQL/Python snippets.

---

## Data Modeling

### 1. Normalization / denormalize (detailed)
1NF atomic values; 2NF no partial-key deps; 3NF no transitive deps. Denormalize for read-heavy analytics where joins cost too much.

### 2. OLTP vs OLAP (easy)
OLTP: normalized, row-store, many small writes. OLAP: star schema, column-store, big aggregations.

### 3. Star vs snowflake (easy)
Star: central fact + denormalized dimensions (fast). Snowflake: normalized dimensions (less redundancy, more joins).

### 4. Data vault (easy)
Hubs (keys), links (relationships), satellites (attributes/history) — auditable, source-agnostic integration.

---

## Database Internals

### 5. MVCC / vacuum / WAL (detailed)
MVCC keeps row versions so readers don't block writers; vacuum reclaims dead tuples; WAL logs changes before applying → crash recovery + replication.

### 6. Covering index (detailed)
Index includes all needed columns → index-only scan. Too many indexes slow writes and bloat storage.
```sql
CREATE INDEX idx ON orders(user_id) INCLUDE (status, total);
```

### 7. Partitioning (easy)
Range (dates), hash (even spread), list (regions) — choose by query/prune pattern.

### 8. Sharding vs partitioning (detailed)
Partitioning splits within one DB; sharding splits across machines by a shard key → horizontal scale, but cross-shard joins/txns are hard.

---

## Pipelines

### 9. Batch vs streaming (easy)
Batch: periodic, high-throughput. Streaming: continuous, low latency (Kafka/Flink).

### 10. ETL vs ELT (easy)
ETL transforms before load; ELT loads raw then transforms in the warehouse (dbt) — flexible, scalable.

### 11. Idempotent pipeline (detailed + code)
Re-running must not duplicate. Use upserts / dedup keys.
```sql
INSERT INTO t (id, val) VALUES (:id, :val)
ON CONFLICT (id) DO UPDATE SET val = EXCLUDED.val;  -- idempotent upsert
```

### 12. Exactly-once vs at-least-once (detailed)
At-least-once may duplicate (retry) → make consumers idempotent. Exactly-once needs transactional offsets/dedup.

### 13. Backfill (easy)
Reprocess historical data through the pipeline; partition by date to backfill safely.

### 14. Schema evolution (detailed)
Add nullable/defaulted fields, use Avro/Protobuf with compatibility rules; avoid breaking renames/removals.

---

## Coding

### 15. Dedup a stream (code)
```python
seen = set()
def process(event):
    if event["id"] in seen: return   # drop duplicate
    seen.add(event["id"]); handle(event)
```

### 16. Window aggregation (code)
```python
from collections import deque
def rolling_sum(stream, window):
    q, s = deque(), 0
    for t, v in stream:
        q.append((t, v)); s += v
        while q and t - q[0][0] > window:
            s -= q.popleft()[1]
        yield s
```

### 17. Star schema query (SQL)
```sql
SELECT d.region, SUM(f.amount)
FROM fact_sales f JOIN dim_store d ON f.store_id = d.id
GROUP BY d.region;
```

---

## Judgment / Failure

### 18. Slow analytics query: row-store + normalized → use columnar warehouse, star schema, pre-aggregation.
### 19. Duplicate records after retry: non-idempotent load → upserts, dedup keys, transactional offsets.
### 20. Pipeline silently drops data: no monitoring/DLQ → add row-count checks, dead-letter queue, alerting.
