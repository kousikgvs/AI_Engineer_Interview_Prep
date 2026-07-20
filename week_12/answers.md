# Week 12 — Answers with Code: Retrieval Science & Advanced RAG

> BM25, hybrid retrieval, rerankers, and chunking with code.

---

## Retrieval Theory

### 1. Why sparse still matters (detailed)
BM25 nails exact matches, rare terms, IDs, out-of-domain queries where dense embeddings miss. Hybrid wins.

### 2. TF-IDF → BM25 (detailed + code)
```python
import numpy as np
def bm25_score(tf, df, N, dl, avgdl, k1=1.5, b=0.75):
    idf = np.log((N - df + 0.5) / (df + 0.5) + 1)
    return idf * (tf * (k1 + 1)) / (tf + k1 * (1 - b + b * dl / avgdl))
```
BM25 adds TF saturation (k1) and length normalization (b) over TF-IDF.

### 3. BM25 vs dense (easy)
BM25 for exact/keyword/cold-start; dense for semantic/paraphrase. Combine.

### 4. Bi- vs cross-encoder (detailed)
Bi-encoder embeds separately (fast, precomputed, less accurate) → retrieve. Cross-encoder jointly attends (accurate, slow) → rerank top-k.

### 5. Hybrid retrieval / RRF (detailed + code)
```python
def rrf(rankings, k=60):
    scores = {}
    for ranking in rankings:            # each is an ordered list of doc ids
        for rank, doc in enumerate(ranking):
            scores[doc] = scores.get(doc, 0) + 1 / (k + rank)
    return sorted(scores, key=scores.get, reverse=True)
```

### 6. ColBERT / late interaction (detailed)
Store per-token embeddings; at query time compute sum of max token-similarities → near cross-encoder accuracy, cheaper.

### 7. HyDE (detailed)
Generate a hypothetical answer, embed it, retrieve against that → bridges short-query/long-doc vocabulary gap.

---

## Chunking & Indexing

### 8. Chunking strategies (detailed)
Fixed-size, sentence/paragraph, semantic, and recursive. Overlap preserves context across boundaries.
```python
def chunk(text, size=512, overlap=64):
    out, i = [], 0
    while i < len(text):
        out.append(text[i:i+size]); i += size - overlap
    return out
```

### 9. Chunk size trade-off (easy)
Small = precise but fragmented; large = context-rich but noisy/diluted retrieval.

### 10. Metadata filtering (easy)
Attach metadata (date, source, permissions) and filter before/after vector search.

### 11. Parent-document / small-to-big (detailed)
Retrieve on small precise chunks but return the larger parent for context.

---

## Reranking & Fusion

### 12. Cross-encoder rerank (code)
```python
pairs = [(query, doc) for doc in candidates]
scores = cross_encoder.predict(pairs)
top = [d for _, d in sorted(zip(scores, candidates), reverse=True)][:k]
```

### 13. MMR (detailed + code)
Balance relevance and diversity to avoid redundant chunks.
```python
def mmr(query_emb, doc_embs, k, lam=0.5):
    selected, cand = [], list(range(len(doc_embs)))
    while len(selected) < k and cand:
        def score(i):
            rel = cos(query_emb, doc_embs[i])
            div = max((cos(doc_embs[i], doc_embs[j]) for j in selected), default=0)
            return lam * rel - (1 - lam) * div
        best = max(cand, key=score); selected.append(best); cand.remove(best)
    return selected
```

---

## Evaluation

### 14. RAG metrics (detailed)
Retrieval: recall@k, MRR, nDCG. Generation: faithfulness, answer relevance, context precision (RAGAS).

### 15. Recall@k / MRR (code)
```python
def recall_at_k(retrieved, relevant, k):
    return len(set(retrieved[:k]) & set(relevant)) / len(relevant)
def mrr(retrieved, relevant):
    for i, d in enumerate(retrieved, 1):
        if d in relevant: return 1 / i
    return 0
```

---

## Coding

### 16. Hybrid + RRF pipeline (see Q5).
### 17. MMR reranking (see Q13).
### 18. Chunker with overlap (see Q8).

---

## Judgment / Failure

### 19. RAG retrieves irrelevant chunks: bad chunking/embeddings → hybrid search, rerank, better chunk size, metadata filters.
### 20. Right doc retrieved but bad answer: lost-in-the-middle or weak prompt → rerank, fewer/ordered chunks, cite sources.
### 21. Retrieval slow at scale: brute-force search → ANN index (HNSW/IVF), quantization, pre-filtering.
