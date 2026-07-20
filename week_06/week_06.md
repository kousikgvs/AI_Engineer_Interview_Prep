# Week 6 — Transformers: Architecture Deep Dive

**Block:** B — Transformers + LLM Internals  
**Goal:** Understand the GPT architecture deeply enough to build it yourself.

---

## What You Will Learn

### The Full Transformer Architecture
- Self-attention — query, key, value; scaled dot-product derivation
- Multi-head attention — why multiple heads, what each head can learn
- The attention mask — causal masking for autoregressive generation
- Feed-forward network — two linear layers with GELU; why the expansion ratio matters
- Residual connections — why they are critical for gradient flow
- Layer normalization — pre-norm vs post-norm, why pre-norm is now the standard
- Encoder vs Decoder vs Encoder-Decoder architectures
- Pretraining objectives: Masked Language Modeling (MLM) vs Causal Language Modeling (CLM)
- GPT vs BERT — decoder-only vs encoder-only, and what each is good at

### Modern GPT Internals
- RMSNorm — a simpler version of LayerNorm, why it is preferred computationally
- Rotary Positional Embeddings (RoPE) — how position is encoded in the query/key space
- SwiGLU activation — why it replaced GeLU in the feed-forward network
- Grouped Query Attention (GQA) — reducing KV cache memory
- KV Cache — what it stores, why it exists, how it speeds up inference
- Flash Attention — the IO-aware attention algorithm (concept and motivation)
- Parameter count derivation — how to calculate GPT-2's parameters from scratch

### Scaling Laws
- Kaplan et al. scaling laws and the Chinchilla insight
- Compute-optimal training
- Why bigger is not always better

### Research Taste Applied
- Why "Attention Is All You Need" changed the field
- What it got wrong (positional encodings evolved, pre-norm is better)
- How to read papers critically, not reverently

---

## Project
- Build GPT from scratch in PyTorch
- Train on Tiny Shakespeare — target coherent text generation
- Implement KV cache and measure the inference speedup
- Calculate GPT-2 parameter counts by hand, then verify

---

## Papers to Read
- Read + reproduce: "Attention Is All You Need" — Vaswani et al. (2017)
- Skim: "Language Models are Few-Shot Learners" — GPT-3 paper

---

## Failure Friday
- Analyze: A production LLM that hallucinated confidently — failure mode analysis at the attention and probability level

---

## Interview Questions This Week Prepares You For
- "Derive scaled dot-product attention from scratch"
- "What is RoPE and why was it invented?"
- "What does the KV cache actually store and why does it matter for inference cost?"

---

## Engineering Judgment Question
**"GPT decoder-only or encoder-decoder for this task?"**  
Write your answer covering: what you would do, why, what tradeoff you are making, and what alternative you rejected.
