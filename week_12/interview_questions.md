# Week 12 — Interview Questions: Retrieval Science & Advanced RAG

> Click a question (▸) to expand the answer.

---

## Retrieval Theory

<details>
<summary>1. Why does sparse retrieval still matter in 2026?</summary>

Keyword/BM25 excels at exact matches, rare terms, IDs, and out-of-domain queries where dense embeddings miss. Hybrid (sparse + dense) beats either alone.
</details>

<details>
<summary>2. Explain TF-IDF then BM25.</summary>

TF-IDF weights terms by frequency in a doc times rarity across the corpus. BM25 adds term-frequency saturation and document-length normalization, making it a stronger, well-tuned ranking function.
</details>

<details>
<summary>3. BM25 vs dense retrieval — when each?</summary>

BM25 for exact/keyword/rare-term and cold-start queries; dense for semantic/paraphrase matches. Combine them (hybrid) for robustness.
</details>

<details>
<summary>4. Bi-encoder vs cross-encoder tradeoff?</summary>

Bi-encoders embed query and docs separately (fast, precomputable, less accurate). Cross-encoders jointly attend to query+doc (accurate, slow). Use bi-encoder to retrieve, cross-encoder to rerank the top-k.
</details>

<details>
<summary>5. What is hybrid retrieval (RRF, weighted fusion)?</summary>

Run sparse and dense retrieval and merge results — Reciprocal Rank Fusion combines rank positions; weighted fusion combines normalized scores — improving recall/precision.
</details>

<details>
<summary>6. What is ColBERT / late interaction?</summary>

It stores per-token embeddings and computes fine-grained max-similarity between query and doc tokens at query time — near cross-encoder accuracy at much lower cost.
</details>

<details>
<summary>7. What is HyDE; why does it improve retrieval?</summary>

The model generates a hypothetical answer to the query, embeds it, and retrieves against that — bridging the vocabulary gap between short queries and full documents.
</details>

<details>
<summary>8. Query decomposition and multi-hop retrieval?</summary>

Break a complex question into sub-queries and retrieve for each, chaining results — needed when the answer requires combining multiple documents.
</details>

## Advanced RAG Architectures

<details>
<summary>9. Naive RAG failure modes?</summary>

Retrieve-once-then-generate fails on ambiguous queries, poor chunks, no reranking, and irrelevant/low-recall retrieval, causing hallucinations or "I don't know."
</details>

<details>
<summary>10. What does advanced RAG add?</summary>

Pre-retrieval (query rewriting/expansion), better retrieval (hybrid), and post-retrieval (reranking, compression, filtering) stages around the core.
</details>

<details>
<summary>11. Self-RAG, CRAG, iterative RAG?</summary>

Self-RAG: the model decides when to retrieve and critiques usefulness. CRAG: evaluates retrieval quality and corrects (e.g., web fallback). Iterative: retrieve → generate → critique → re-retrieve until confident.
</details>

<details>
<summary>12. Knowledge graph / GraphRAG vs vector DB?</summary>

Use graphs for structured, multi-hop, relationship-heavy reasoning and global summarization; vector DBs for semantic passage lookup. GraphRAG builds an entity/relationship graph over the corpus.
</details>

## Reranking & Chunking

<details>
<summary>13. What does a cross-encoder reranker do?</summary>

It re-scores the top-k retrieved docs by jointly attending to query+doc, catching relevance that separate embeddings miss — a big precision boost on a small candidate set.
</details>

<details>
<summary>14. RRF vs MMR — when want diversity?</summary>

RRF fuses rankings from multiple retrievers. MMR (Maximal Marginal Relevance) balances relevance with novelty to avoid near-duplicate results when you need diverse coverage.
</details>

<details>
<summary>15. Fixed vs semantic vs recursive chunking — when fail?</summary>

Fixed can cut mid-idea; semantic groups related sentences; recursive respects document structure. Fixed fails when an answer spans a boundary; too-large chunks dilute relevance.
</details>

<details>
<summary>16. Parent-child chunk indexing; chunk overlap?</summary>

Index small child chunks for precise retrieval but return the larger parent for context. Overlap prevents losing information at boundaries but adds redundancy/cost.
</details>

## Evaluation

<details>
<summary>17. Evaluate retrieval without labeled data?</summary>

Use LLM-as-judge for relevance, synthetic query-doc pairs, faithfulness/groundedness checks, and proxy signals (answer correctness, "I don't know" rate); collect user feedback over time.
</details>

<details>
<summary>18. Define NDCG, MRR, Hit@K.</summary>

Hit@K: is a relevant doc in the top K. MRR: average reciprocal rank of the first relevant doc. NDCG: graded relevance discounted by position — rewards ranking relevant docs higher.
</details>

## System Design

<details>
<summary>19. Design a production RAG system.</summary>

Ingest → chunk → embed → hybrid index (BM25 + vector). Query: rewrite/HyDE → hybrid retrieve → cross-encoder rerank → prompt with sources + citations → generate → evaluate faithfulness; add caching, monitoring, and re-index on updates.
</details>

## Coding

<details>
<summary>20. Build BM25 + dense hybrid; benchmark vs HyDE/reranking.</summary>

Retrieve with both, fuse via RRF, optionally rerank; compare naive vs HyDE vs reranking using NDCG/MRR/Hit@K on a labeled or LLM-judged set.
</details>

## Failure / Judgment

<details>
<summary>21. RAG failure from chunking caused hallucinations.</summary>

Context split across chunks meant the retriever returned partial info, so the model filled gaps by inventing. Fix chunking (semantic/parent-child), add reranking, and require grounded citations.
</details>

<details>
<summary>22. "BM25, dense, or hybrid retrieval?"</summary>

**What I'd do:** hybrid by default — dense for semantics, BM25 for exact/rare terms, fused with RRF.
**Why:** it's the most robust across query types.
**Tradeoff:** more moving parts and latency.
**Rejected:** pure dense (misses exact matches) or pure BM25 (misses paraphrase) unless the domain clearly favors one.
</details>
