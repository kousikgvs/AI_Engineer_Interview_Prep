# Week 12 — Retrieval Science & Advanced RAG

**Block:** D — LLM Engineering + RAG + Agents  
**Goal:** Most RAG courses teach vector DBs. That is shallow. Production RAG fails at retrieval.

---

## What You Will Learn

### Retrieval Theory
- Why sparse retrieval still matters even in 2026
- TF-IDF — derivation and intuition
- BM25 — the gold standard sparse retrieval algorithm with full formula derivation
- Dense retrieval — bi-encoder models like DPR, E5, BGE
- Cross-encoders — reranking, why they are more accurate but slower
- Hybrid retrieval — combining sparse and dense scores (RRF and weighted fusion)
- ColBERT — late interaction for efficient and accurate retrieval
- Query expansion — HyDE (Hypothetical Document Embeddings)
- Query decomposition — breaking complex questions into sub-queries
- Multi-hop retrieval — chaining multiple retrievals for complex questions

### Advanced RAG Architectures
- Naive RAG — retrieve once, generate (and why it fails)
- Advanced RAG — pre-retrieval, retrieval, and post-retrieval processing
- Modular RAG — treating retrieval as configurable modules
- Self-RAG — the model decides when to retrieve and how to use results
- Corrective RAG (CRAG) — evaluating and fixing retrieval quality
- Iterative RAG — retrieve → generate → critique → re-retrieve loop

### Knowledge Graphs
- Graph-based knowledge representation
- GraphRAG — Microsoft's approach
- Combining structured knowledge with vector retrieval
- When to use knowledge graphs vs vector DBs

### Reranking
- Cross-encoder reranking — full query-document attention
- Cohere Rerank, Jina Reranker
- Reciprocal Rank Fusion (RRF)
- Diversity in reranking — Maximum Marginal Relevance (MMR)

### Chunking Science
- Fixed-size chunking — when it fails
- Semantic chunking using sentence transformers + similarity threshold
- Recursive character chunking
- Document-level vs passage-level vs sentence-level chunks
- Chunk overlap and its effects
- Parent-child chunk indexing

---

## Project
- Build a full RAG pipeline with BM25 + dense retrieval hybrid
- Implement and benchmark: naive RAG vs HyDE vs reranking
- Evaluate retrieval quality using NDCG, MRR, Hit@K
- Build a simple GraphRAG pipeline on a knowledge-rich dataset

---

## Papers to Read
- "REALM: Retrieval-Augmented Language Model Pre-Training" (concept level)
- "HyDE: Precise Zero-Shot Dense Retrieval without Relevance Labels" — Gao et al.

---

## Failure Friday
- Analyze: A production RAG failure traced to chunking decisions — hallucinations caused by splitting context across chunks

---

## Interview Questions This Week Prepares You For

<details>
<summary>"Compare BM25 and dense retrieval — when would you use each?"</summary>

BM25 for exact/keyword/rare-term and cold-start queries; dense for semantic/paraphrase matches. Hybrid (fused) is the most robust default.
</details>

<details>
<summary>"What is HyDE and why does it improve retrieval?"</summary>

The model generates a hypothetical answer, embeds it, and retrieves against that — bridging the vocabulary gap between a short query and full documents.
</details>

<details>
<summary>"How would you evaluate your retrieval system without labeled data?"</summary>

LLM-as-judge relevance, synthetic query-doc pairs, groundedness/faithfulness checks, proxy signals (answer correctness, "I don't know" rate), and user feedback over time.
</details>

---

## Engineering Judgment Question

<details>
<summary><strong>"BM25, dense retrieval, or hybrid retrieval for this use case?"</strong></summary>

**What I'd do:** hybrid by default — dense for semantics + BM25 for exact/rare terms, fused via RRF.
**Why:** most robust across query types.
**Tradeoff:** more components and latency.
**Rejected:** pure dense (misses exact matches) or pure BM25 (misses paraphrase) unless the domain clearly favors one.
</details>
