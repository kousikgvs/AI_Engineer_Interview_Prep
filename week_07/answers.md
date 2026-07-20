# Week 7 — Answers with Code: Inference, Quantization & Fine-Tuning

> Sampling, precision, and PEFT/LoRA math with code.

---

## Inference

### 1. Autoregressive generation (code)
```python
def generate(model, ids, n, temperature=1.0):
    for _ in range(n):
        logits = model(ids)[:, -1]          # last-position logits
        probs = softmax(logits / temperature)
        nxt = sample(probs)
        ids = np.concatenate([ids, [[nxt]]], axis=1)
    return ids
```

### 2. Temperature / top-k / top-p (code)
```python
def sample_logits(logits, temp=1.0, top_k=0, top_p=1.0):
    logits = logits / temp
    if top_k:
        kth = np.sort(logits)[-top_k]
        logits = np.where(logits < kth, -1e9, logits)
    probs = softmax(logits)
    if top_p < 1.0:
        idx = np.argsort(-probs)
        cum = np.cumsum(probs[idx])
        cut = idx[cum > top_p][1:]           # drop tail beyond p
        probs[cut] = 0; probs /= probs.sum()
    return np.random.choice(len(probs), p=probs)
```
Temp scales randomness; top-k caps candidates; top-p adapts the set size.

### 3. Greedy vs beam vs sampling (easy)
Greedy=argmax (repetitive); beam keeps N sequences (translation); sampling adds diversity.

### 4. Repetition penalty (code)
```python
for t in generated_tokens:
    logits[t] /= penalty  # penalty>1 discourages repeats
```

### 5. Speculative decoding (detailed)
Draft model proposes k tokens; target verifies in one pass, accepts longest correct prefix. Preserves target distribution, cuts latency 2–3×.

### 6. Continuous batching (easy)
Add/remove requests each step so the GPU never idles (vLLM).

### 7. PagedAttention (detailed)
KV cache in fixed non-contiguous pages (like OS virtual memory) → no fragmentation, sharing across requests, more concurrent sequences.

---

## Quantization

### 8. FP32/FP16/BF16 (detailed)
sign + exponent(range) + mantissa(precision). BF16 = FP32 exponent range, fewer mantissa bits.

### 9. Why BF16 for training (detailed)
Same dynamic range as FP32 → no overflow/underflow of gradients; FP16 needs loss scaling.

### 10. INT8 / GPTQ / AWQ (detailed)
Post-training quantization to int8/int4. GPTQ minimizes layerwise reconstruction error; AWQ protects salient (activation-aware) weight channels. ~4× smaller, minor quality loss.

### 11. Quantization dequant math (code)
```python
scale = (x.max() - x.min()) / 255
q = np.round((x - x.min()) / scale).astype(np.uint8)
x_hat = q * scale + x.min()   # dequantize
```

### 12. QLoRA (detailed)
4-bit (NF4) frozen base + LoRA adapters in bf16 → fine-tune 65B on one GPU.

---

## Fine-Tuning (LoRA/PEFT)

### 13. LoRA math (detailed + code)
Freeze W; learn low-rank update ΔW = BA (r ≪ d). Trains ~0.1% of params.
```python
# y = x W + (x A) B * (alpha / r)
class LoRA(nn.Module):
    def __init__(self, W, r, alpha):
        super().__init__(); self.W = W.requires_grad_(False)
        d_in, d_out = W.shape
        self.A = nn.Parameter(torch.randn(d_in, r) * 0.01)
        self.B = nn.Parameter(torch.zeros(r, d_out))
        self.s = alpha / r
    def forward(self, x):
        return x @ self.W + (x @ self.A @ self.B) * self.s
```
B init to zero → training starts as the base model.

### 14. Rank r trade-off (easy)
Higher r = more capacity/params; low r may underfit complex tasks.

### 15. Full FT vs LoRA (detailed)
Full: all weights, best quality, costly, one copy per task. LoRA: tiny adapters, swappable, cheap, slightly less capacity.

### 16. Catastrophic forgetting (easy)
Fine-tuning overwrites pretrained knowledge; mitigate with low LR, adapters, replay data.

---

## Coding

### 17. Implement sampling (see Q2).
### 18. LoRA layer (see Q13).
### 19. Quantize a tensor (see Q11).

---

## Judgment / Failure

### 20. When NOT to fine-tune: if prompting/RAG suffices, or data is scarce/changing — fine-tuning is costly and freezes knowledge.
### 21. LoRA underperforms: rank too low, wrong target modules (add q/k/v/o + FFN), or LR too small.
### 22. Quantized model degrades: outlier activations; use AWP/AWQ, keep sensitive layers higher precision, or per-channel scales.
