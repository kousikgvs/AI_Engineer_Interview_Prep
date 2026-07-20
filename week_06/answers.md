# Week 6 — Answers with Code: Transformers Architecture Deep Dive

> The most-asked LLM topic. Full attention implementation + derivations.

---

## Core Transformer

### 1. Scaled dot-product attention (full derivation + code)
`Attention(Q,K,V) = softmax(QKᵀ/√d_k) V`. Divide by √d_k so dot products don't grow with dimension and saturate softmax.
```python
import numpy as np
def attention(Q, K, V, mask=None):
    d_k = Q.shape[-1]
    scores = Q @ K.transpose(0, 2, 1) / np.sqrt(d_k)
    if mask is not None:
        scores = np.where(mask, scores, -1e9)   # causal mask
    w = np.exp(scores - scores.max(-1, keepdims=True))
    w = w / w.sum(-1, keepdims=True)             # softmax
    return w @ V
```

### 2. Q/K/V (easy)
Learned projections: query asks, keys advertise, values carry content.

### 3. Multi-head (detailed)
Each head attends in a different subspace (syntax, coreference, position); outputs concat + mix.
```python
def multihead(x, Wq, Wk, Wv, Wo, n_heads):
    B, T, D = x.shape; h = D // n_heads
    q = (x @ Wq).reshape(B, T, n_heads, h).transpose(0,2,1,3)
    k = (x @ Wk).reshape(B, T, n_heads, h).transpose(0,2,1,3)
    v = (x @ Wv).reshape(B, T, n_heads, h).transpose(0,2,1,3)
    out = np.stack([attention(q[:,i], k[:,i], v[:,i]) for i in range(n_heads)], 1)
    return out.transpose(0,2,1,3).reshape(B, T, D) @ Wo
```

### 4. Causal mask (detailed)
```python
mask = np.tril(np.ones((T, T))).astype(bool)   # lower-triangular: no future
```

### 5. FFN expand/GELU (easy)
Project up (×4), GELU, project down — per-token capacity/mixing.

### 6. Residuals (detailed)
Skip connections give gradients a direct path so deep stacks train; each block learns a residual.

### 7. Pre-norm vs post-norm (detailed)
Pre-norm (LayerNorm before sublayer) keeps residual path clean and gradients stable → trains without careful warmup; now standard.

### 8. Encoder/decoder/enc-dec (easy)
BERT (bidirectional), GPT (causal), T5 (seq2seq).

### 9. MLM vs CLM (easy)
Mask-and-predict (understanding) vs next-token (generation).

---

## Modern GPT Internals

### 10. RMSNorm (code)
```python
def rmsnorm(x, gamma, eps=1e-6):
    return gamma * x / np.sqrt((x**2).mean(-1, keepdims=True) + eps)
```
No mean subtraction/bias → cheaper, as good.

### 11. RoPE (detailed)
Rotates Q/K by an angle ∝ position, encoding relative position directly in attention; extrapolates to longer contexts better than absolute encodings.

### 12. SwiGLU (easy)
Gated activation (Swish × linear gate) in the FFN; better quality at similar compute.

### 13. GQA (detailed)
Share K/V heads across groups of query heads → smaller KV cache / bandwidth with minimal quality loss (between MHA and MQA).

### 14. KV cache (detailed)
Caches past keys/values so each new token attends in O(seq), not recomputing O(seq²). Dominates inference memory (∝ context × batch).

### 15. Flash Attention (detailed)
Tiles Q/K/V in SRAM + online softmax; never materializes the N×N matrix in HBM → O(N) memory, faster.

### 16. GPT-2 param count (derivation)
Per layer ≈ 12·d² (attention 4d² + FFN 8d²); total = L·12d² + vocab·d + norms.

---

## Scaling

### 17. Kaplan vs Chinchilla (detailed)
Kaplan: loss ~ power law in params/data/compute. Chinchilla: for a compute budget, use more data + smaller model (~20 tokens/param).

### 18. Compute-optimal (easy)
Optimal model/data balance per budget; oversized-undertrained wastes compute.

---

## Coding

### 19. GPT from scratch (PyTorch sketch)
```python
import torch.nn as nn
class Block(nn.Module):
    def __init__(self, d, h):
        super().__init__()
        self.ln1 = nn.LayerNorm(d); self.attn = nn.MultiheadAttention(d, h, batch_first=True)
        self.ln2 = nn.LayerNorm(d)
        self.ff = nn.Sequential(nn.Linear(d,4*d), nn.GELU(), nn.Linear(4*d,d))
    def forward(self, x, mask):
        a,_ = self.attn(self.ln1(x), self.ln1(x), self.ln1(x), attn_mask=mask)
        x = x + a
        return x + self.ff(self.ln2(x))
```

### 20. KV cache speedup: store per-layer K/V, append each step; measure tokens/sec vs no cache.

---

## Judgment / Failure

### 21. Decoder-only vs enc-dec: decoder-only for open generation/chat; enc-dec for translation/summarization.
### 22. Confident hallucination: model samples highest-probability (not factual) continuation; mitigate with RAG, low temp, calibration, verification.
