# Week 10 — Interview Questions: Data Engineering

> Sourced from data-engineer + AI-platform screens. "Most AI projects fail on data, not models."

---

## Data Modeling
1. What is normalization (1NF/2NF/3NF)? When do you denormalize?
2. OLTP vs OLAP design patterns.
3. Star schema vs snowflake schema — tradeoffs.
4. What is data vault modeling and when is it useful?

## Database Internals
5. Explain Postgres MVCC, vacuum, and the WAL.
6. What is a covering index? When does query optimization backfire?
7. Range vs hash vs list partitioning — when to use each.
8. When do you use JSONB in Postgres?

## Pipelines & Streaming
9. ETL vs ELT — when does each make sense?
10. What is an Airflow DAG? Operators, sensors, scheduling, backfills.
11. How do you enforce data quality (great_expectations, dbt tests)?
12. Explain Kafka: partitions, consumer groups, offsets.
13. **What is Change Data Capture (CDC) and why does it matter for AI systems?**
14. Flink vs Spark Streaming — tradeoffs. What is windowing?

## Feature Stores & Embeddings
15. What is a feature store and what is point-in-time correctness?
16. Online vs offline feature serving.
17. Chunking strategies: fixed vs semantic vs recursive — tradeoffs.
18. What happens when you change your embedding model (versioning)?
19. Batch vs online embedding generation.

## Vector DB Internals
20. **Explain HNSW — how does approximate nearest-neighbor search work?**
21. What is IVF-PQ?
22. Pgvector vs Pinecone vs Weaviate vs Qdrant vs FAISS vs Chroma vs Milvus — how do you choose?
23. ANN vs exact search — the accuracy/latency tradeoff.
24. How do you filter on metadata alongside vector search?

## System Design
25. **Design an embedding pipeline that handles 10 million documents.**

## Failure / Judgment
26. A production RAG system had poor retrieval — trace it to chunking/embedding decisions.
27. "Kafka or SQS for this pipeline?" — defend it.
