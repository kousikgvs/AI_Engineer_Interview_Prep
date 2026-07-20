# Week 6 — Interview Questions: Transformers Architecture Deep Dive

> Click a question (▸) to expand the answer.

---

## Core Transformer

<details>
<summary>1. Derive scaled dot-product attention; why √d_k?</summary>

Attention(Q,K,V) = softmax(QKᵀ/√d_k)V. Q·K scores similarity; softmax turns it into weights; weighted sum of V is the output. Dividing by √d_k keeps dot products from growing with dimension, which would saturate softmax and kill gradients.
</details>

<details>
<summary>2. What are query, key, value?</summary>

Learned projections of each token: the query asks "what do I need," keys advertise "what I offer," values carry the content. A token gathers values weighted by query-key match.
</details>

<details>
<summary>3. Multi-head attention — why multiple heads?</summary>

Each head attends in a different learned subspace, capturing different relations (syntax, coreference, position). Their outputs concatenate and mix, giving richer representations than one head.
</details>

<details>
<summary>4. What is the causal mask; why needed?</summary>

It sets attention to future positions to −∞ before softmax so each token only sees past/current tokens — required for autoregressive generation to avoid cheating.
</details>

<details>
<summary>5. Why FFN expands then contracts; why GELU?</summary>

The FFN projects up (e.g., 4×), applies non-linearity, projects down — adding capacity/mixing per token. GELU is a smooth gating non-linearity that outperforms ReLU here.
</details>

<details>
<summary>6. Why are residual connections critical?</summary>

They give gradients a direct path around each block, preventing vanishing gradients and letting very deep transformers train; each block learns a residual refinement.
</details>

<details>
<summary>7. Pre-norm vs post-norm; why pre-norm standard?</summary>

Pre-norm applies LayerNorm before the sublayer, keeping the residual path clean and gradients stable, so deep models train without careful warmup — hence it's now standard.
</details>

<details>
<summary>8. Encoder-only vs decoder-only vs encoder-decoder?</summary>

Encoder-only (BERT): bidirectional, for understanding/classification. Decoder-only (GPT): causal, for generation. Encoder-decoder (T5): for seq2seq like translation.
</details>

<details>
<summary>9. MLM vs CLM; GPT vs BERT?</summary>

MLM (BERT) masks tokens and predicts them bidirectionally (understanding). CLM (GPT) predicts the next token left-to-right (generation). Different objectives, different strengths.
</details>

## Modern GPT Internals

<details>
<summary>10. What is RMSNorm; why preferred?</summary>

Normalizes by root-mean-square only (no mean subtraction, no bias), so it's cheaper and empirically as good — common in modern LLMs (LLaMA).
</details>

<details>
<summary>11. What is RoPE; why invented?</summary>

Rotary Positional Embedding rotates Q/K by an angle proportional to position, encoding relative position directly in attention. It generalizes better to longer contexts than absolute encodings.
</details>

<details>
<summary>12. What is SwiGLU; why replace GeLU?</summary>

A gated activation (Swish × linear gate) in the FFN that improves quality at similar compute — used in modern LLMs.
</details>

<details>
<summary>13. What is GQA; what problem does it solve?</summary>

Grouped Query Attention shares key/value heads across groups of query heads, shrinking the KV cache and memory bandwidth at inference with minimal quality loss (between MHA and MQA).
</details>

<details>
<summary>14. What does the KV cache store; why matter for cost?</summary>

It caches keys/values for all past tokens so each new token's attention is O(seq) not O(seq²) recomputation. It dominates inference memory and grows with context length × batch.
</details>

<details>
<summary>15. What is Flash Attention; why IO-aware?</summary>

An exact attention algorithm that tiles Q/K/V in fast SRAM and uses online softmax to avoid materializing the full N×N matrix in slow HBM — cutting memory from O(N²) to O(N) and speeding it up.
</details>

<details>
<summary>16. Derive GPT-2 parameter count.</summary>

Sum embeddings (vocab×d), and per layer: attention (4·d²) + FFN (8·d²) ≈ 12·d² per layer, times L layers, plus norms. Plug in d, L, vocab to get the total.
</details>

## Scaling

<details>
<summary>17. Kaplan vs Chinchilla scaling laws?</summary>

Kaplan showed loss scales as a power law in params/data/compute. Chinchilla revised the compute-optimal split: for a fixed budget, use more data and a smaller model than Kaplan implied (~20 tokens/param).
</details>

<details>
<summary>18. Compute-optimal training; why isn't bigger always better?</summary>

For a fixed compute budget, there's an optimal model/data balance; an oversized under-trained model wastes compute and underperforms a smaller well-trained one.
</details>

## Coding

<details>
<summary>19. Build a GPT from scratch; train Tiny Shakespeare.</summary>

Implement token+positional embeddings, multi-head causal self-attention, pre-norm + residual FFN blocks, and a language-model head; train with next-token cross-entropy.
</details>

<details>
<summary>20. Implement a KV cache; measure speedup.</summary>

Store per-layer K/V for generated tokens and append each step instead of recomputing; measure tokens/sec vs no cache to show the large decoding speedup.
</details>

## Judgment / Failure

<details>
<summary>21. "Decoder-only or encoder-decoder for this task?"</summary>

Decoder-only for open-ended generation and chat (simpler, scales well); encoder-decoder for strict input→output mapping like translation/summarization. Tradeoff: encoder-decoder separates understanding from generation; rejected it when a single causal stack suffices.
</details>

<details>
<summary>22. LLM hallucinated confidently — analyze at attention/probability level.</summary>

The model samples the highest-probability continuation, which need not be factual; attention may latch onto spurious context. Mitigate with grounding (RAG), lower temperature, calibration, and verification.
</details>
