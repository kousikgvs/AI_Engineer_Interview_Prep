# Week 10 — Data Engineering

**Block:** C — Systems + Backend + Data Engineering  
**Goal:** Most real AI projects fail because of data problems, not model problems. Fix that.

---

## What You Will Learn

### Data Modeling
- Relational data modeling — normalization, 1NF/2NF/3NF
- OLTP vs OLAP design patterns
- Star schema and snowflake schema
- Data vault modeling for historical tracking

### Database Internals
- Postgres internals — storage, MVCC, vacuum, WAL
- Query optimization — indexes, query plans, covering indexes
- Partitioning — range, hash, and list partitioning
- JSONB in Postgres — when to use it

### Data Pipelines
- ETL vs ELT — tradeoffs and when each approach makes sense
- Apache Airflow — DAGs, operators, scheduling, and sensors
- Data quality checks — great_expectations and dbt testing
- Schema evolution and migration strategies

### Streaming Data
- Event-driven architecture — producers, consumers, topics
- Apache Kafka — partitions, consumer groups, offsets
- Change Data Capture (CDC) — tracking changes in source systems with Debezium
- Stream processing — filtering, aggregation, windowing
- Flink vs Spark Streaming

### Feature Engineering & Feature Stores
- Feature engineering for ML — encoding, imputation, scaling
- Feature Store concepts — point-in-time correctness
- Online vs offline feature serving
- Feast and Tecton (concept level)

### Embedding Pipelines
- Chunking strategies — fixed size, semantic, recursive
- Embedding model selection — sentence-transformers, OpenAI embeddings
- Batch vs online embedding generation
- Embedding versioning — what happens when you change models?

### Vector Database Internals
- HNSW — Hierarchical Navigable Small World (the algorithm behind fast ANN search)
- IVF-PQ — Inverted File Index with Product Quantization
- Pgvector vs Pinecone vs Weaviate vs Qdrant — architectural tradeoffs
- FAISS and Chroma and Milvus — when to reach for each
- Approximate nearest neighbor (ANN) vs exact search tradeoffs
- Filtering on metadata combined with vector search

---

## Project
Full pipeline from raw data to retrieval:
1. Raw Data (CSV/JSON/Postgres)
2. Cleaning & Validation (Airflow DAG)
3. Feature Engineering
4. Embedding Generation (batch)
5. Vector DB Ingestion
6. Retrieval API

- Deploy Kafka locally and implement a CDC pipeline
- Build a feature store for a tabular ML dataset
- Ship an embedding pipeline with chunking + Pgvector

---

## Paper to Read
- "Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks" — Lewis et al. (2020)

---

## Failure Friday
- Analyze: A production RAG system with poor retrieval — trace the failure back to chunking and embedding pipeline decisions

---

## Interview Questions This Week Prepares You For
- "Design an embedding pipeline that handles 10 million documents"
- "What is Change Data Capture and why does it matter for AI systems?"
- "Explain HNSW — how does approximate nearest neighbor search work?"

---

## Engineering Judgment Question
**"Kafka or SQS for this pipeline?"**  
Write your answer covering: what you would do, why, what tradeoff you are making, and what alternative you rejected.
