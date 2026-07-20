# Week 12 — Interview Questions: Retrieval Science & Advanced RAG

> Sourced from RAG-heavy applied-AI screens. They probe past "just use a vector DB."

---

## Retrieval Theory
1. Why does sparse retrieval still matter in 2026?
2. Derive/explain TF-IDF and then BM25. What does BM25 add over TF-IDF?
3. **Compare BM25 and dense retrieval — when would you use each?**
4. Bi-encoder vs cross-encoder — accuracy vs latency tradeoff.
5. What is hybrid retrieval? How do RRF and weighted fusion combine scores?
6. What is ColBERT / late interaction?
7. **What is HyDE and why does it improve retrieval?**
8. Query decomposition and multi-hop retrieval — when are they needed?

## Advanced RAG Architectures
9. Naive RAG — what are its failure modes?
10. What does "advanced RAG" add (pre/retrieval/post-retrieval processing)?
11. What is Self-RAG? Corrective RAG (CRAG)? Iterative RAG?
12. When do you use a knowledge graph / GraphRAG vs a vector DB?

## Reranking & Chunking
13. What does a cross-encoder reranker do that retrieval can't?
14. What is Reciprocal Rank Fusion (RRF)? What is MMR and when do you want diversity?
15. Fixed vs semantic vs recursive chunking — when does each fail?
16. What is parent-child chunk indexing? How does chunk overlap help/hurt?

## Evaluation
17. **How would you evaluate a retrieval system without labeled data?**
18. Define NDCG, MRR, and Hit@K.

## System Design
19. **Design a production RAG system** with hybrid retrieval + reranking + eval.

## Coding
20. Build BM25 + dense hybrid; benchmark naive RAG vs HyDE vs reranking.

## Failure / Judgment
21. A RAG failure traced to chunking caused hallucinations from split context — explain and fix.
22. "BM25, dense, or hybrid retrieval for this use case?" — defend it.
