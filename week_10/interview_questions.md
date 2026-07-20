# Week 10 — Interview Questions: Data Engineering

> Click a question (▸) to expand the answer.

---

## Data Modeling

<details>
<summary>1. What is normalization (1NF/2NF/3NF); when denormalize?</summary>

Organizing data to remove redundancy (each fact stored once). 1NF: atomic values; 2NF: no partial key dependencies; 3NF: no transitive dependencies. Denormalize for read performance when joins get too costly (analytics/warehouses).
</details>

<details>
<summary>2. OLTP vs OLAP design patterns?</summary>

OLTP: normalized, row-oriented, many small writes. OLAP: denormalized star/snowflake schemas, column-oriented, big aggregations.
</details>

<details>
<summary>3. Star vs snowflake schema?</summary>

Star: a central fact table with denormalized dimension tables (simple, fast). Snowflake: dimensions further normalized (less redundancy, more joins).
</details>

<details>
<summary>4. What is data vault modeling?</summary>

A pattern (hubs, links, satellites) for auditable, historized data integration that scales to many sources and tracks change over time.
</details>

## Database Internals

<details>
<summary>5. Explain Postgres MVCC, vacuum, WAL.</summary>

MVCC keeps row versions so readers don't block writers. Vacuum reclaims dead versions. WAL (write-ahead log) records changes before applying them, enabling crash recovery and replication.
</details>

<details>
<summary>6. Covering index; when does optimization backfire?</summary>

A covering index includes all columns a query needs, so it's answered from the index alone. Too many indexes slow writes and bloat storage; wrong indexes mislead the planner.
</details>

<details>
<summary>7. Range vs hash vs list partitioning?</summary>

Range: by value ranges (dates). Hash: by hash for even distribution. List: by explicit value sets (regions). Choose by how you query/prune.
</details>

<details>
<summary>8. When use JSONB in Postgres?</summary>

For semi-structured/variable data you want to store and query flexibly (with GIN indexes) — but relational columns are better for stable, heavily-queried fields.
</details>

## Pipelines & Streaming

<details>
<summary>9. ETL vs ELT — when each?</summary>

ETL transforms before loading (good for strict schemas/limited storage). ELT loads raw then transforms in a powerful warehouse (flexible, modern default with cloud warehouses).
</details>

<details>
<summary>10. Airflow DAG: operators, sensors, scheduling, backfills?</summary>

A DAG defines task dependencies. Operators run work; sensors wait for conditions; the scheduler runs on a cadence; backfills rerun past intervals.
</details>

<details>
<summary>11. Enforce data quality (great_expectations, dbt tests)?</summary>

Define expectations/tests (nulls, ranges, uniqueness, referential integrity) that run in the pipeline and fail/alert on violations before bad data propagates.
</details>

<details>
<summary>12. Kafka: partitions, consumer groups, offsets?</summary>

Topics split into partitions for parallelism; consumer groups divide partitions among members; offsets track each consumer's position so it can resume and process exactly/at-least once.
</details>

<details>
<summary>13. What is CDC; why matter for AI systems?</summary>

Change Data Capture streams row-level changes from source DBs (via the WAL) so downstream systems — like an embedding index — stay fresh in near-real-time instead of batch reprocessing.
</details>

<details>
<summary>14. Flink vs Spark Streaming; windowing?</summary>

Flink is true low-latency streaming (event-at-a-time); Spark Streaming is micro-batch. Windowing groups events by time (tumbling/sliding/session) for aggregation.
</details>

## Feature Stores & Embeddings

<details>
<summary>15. Feature store; point-in-time correctness?</summary>

A central store serving consistent features for training and serving. Point-in-time correctness means training features use only data available at that historical moment — preventing label leakage.
</details>

<details>
<summary>16. Online vs offline feature serving?</summary>

Offline: batch features for training (warehouse). Online: low-latency features for real-time inference (key-value store). The store keeps them consistent.
</details>

<details>
<summary>17. Chunking: fixed vs semantic vs recursive — when fail?</summary>

Fixed splits by size (can cut mid-thought). Semantic splits on meaning/similarity. Recursive splits by structure (paragraphs→sentences). Fixed fails when it breaks coherent context needed to answer.
</details>

<details>
<summary>18. What happens when you change the embedding model?</summary>

Old and new vectors are incompatible, so you must re-embed the whole corpus and version the index; mixing versions corrupts similarity search.
</details>

<details>
<summary>19. Batch vs online embedding generation?</summary>

Batch for bulk ingestion (throughput). Online for fresh, per-request items (latency). Many systems do both.
</details>

## Vector DB Internals

<details>
<summary>20. Explain HNSW.</summary>

A multi-layer proximity graph: search starts at a sparse top layer and descends, greedily hopping to nearer neighbors. It gives fast, high-recall approximate nearest-neighbor search in log-ish time.
</details>

<details>
<summary>21. What is IVF-PQ?</summary>

Inverted File clusters vectors into cells (search only nearby cells); Product Quantization compresses vectors into codebooks for memory-efficient approximate distance — scales to billions of vectors.
</details>

<details>
<summary>22. Pgvector vs Pinecone vs Weaviate vs Qdrant vs FAISS vs Chroma vs Milvus?</summary>

Pgvector: vectors inside Postgres (simple). Pinecone: managed SaaS. Weaviate/Qdrant/Milvus: dedicated scalable vector DBs. FAISS: in-process library. Chroma: lightweight local/dev. Choose by scale, ops burden, and metadata/filtering needs.
</details>

<details>
<summary>23. ANN vs exact search tradeoff?</summary>

Exact is 100% accurate but O(n) and slow at scale; ANN trades a little recall for huge speed/memory gains via graphs/quantization. Production uses ANN with a tuned recall target.
</details>

<details>
<summary>24. Filtering metadata alongside vector search?</summary>

Combine vector similarity with structured filters (date, source, tenant). Pre-filtering narrows candidates first; post-filtering removes after search — engines optimize this to keep recall high.
</details>

## System Design

<details>
<summary>25. Design an embedding pipeline for 10M documents.</summary>

Ingest → clean/validate (Airflow) → chunk → batch-embed (GPU workers, dedupe/version) → upsert into a vector DB (HNSW) with metadata → serve a retrieval API; add CDC for updates, monitoring, and re-embed strategy on model change.
</details>

## Failure / Judgment

<details>
<summary>26. RAG had poor retrieval — trace to chunking/embedding.</summary>

Bad chunking split answers across chunks or made them too large/small; wrong embedding model or no reranking hurt relevance. Fix chunking strategy, embedding choice, and add hybrid + reranking; measure with retrieval metrics.
</details>

<details>
<summary>27. "Kafka or SQS for this pipeline?"</summary>

**What I'd do:** Kafka for high-throughput, ordered, replayable event streams with multiple consumers; SQS for simple, managed, decoupled task queues.
**Why:** Kafka retains an event log; SQS is zero-ops.
**Tradeoff:** Kafka has operational complexity.
**Rejected:** Kafka when you just need a simple managed queue.
</details>
