# Week 6 — Interview Questions: Transformers Architecture Deep Dive

> The single most-asked LLM interview topic. Sourced from OpenAI/Anthropic/Cursor-style screens. You must be able to derive attention.

---

## Core Transformer
1. **Derive scaled dot-product attention from scratch.** Why divide by √d_k?
2. What are query, key, and value — intuitively and in matrix form?
3. What is multi-head attention and why use multiple heads? What does each head learn?
4. What is the causal/attention mask and why is it needed for autoregressive generation?
5. Why does the feed-forward network expand then contract (expansion ratio)? Why GELU?
6. Why are residual connections critical for training deep transformers?
7. Pre-norm vs post-norm LayerNorm — why is pre-norm now standard?
8. Encoder-only vs decoder-only vs encoder-decoder — when to use each?
9. MLM vs CLM pretraining objectives — GPT vs BERT.

## Modern GPT Internals
10. What is RMSNorm and why is it preferred over LayerNorm?
11. **What is RoPE and why was it invented?** How does it encode position in Q/K space?
12. What is SwiGLU and why did it replace GeLU in the FFN?
13. What is Grouped Query Attention (GQA) and what problem does it solve?
14. **What does the KV cache store and why does it matter for inference cost?**
15. What is Flash Attention (conceptually) and why is it IO-aware?
16. Derive the parameter count of GPT-2 by hand.

## Scaling
17. What do the Kaplan scaling laws say? What did Chinchilla revise?
18. What is compute-optimal training? Why isn't "bigger always better"?

## Coding
19. Build a GPT from scratch in PyTorch; train on Tiny Shakespeare.
20. Implement a KV cache and measure the inference speedup.

## Judgment / Failure
21. "Decoder-only or encoder-decoder for this task?" — defend it.
22. A production LLM hallucinated confidently — analyze at the attention/probability level.
