# Week 7 — Interview Questions: Inference, Quantization & Fine-Tuning

> Sourced from AI-infra and applied-LLM screens (Together AI, Fireworks, Groq style). Very hot topic in 2026.

---

## Inference
1. How does autoregressive generation work token-by-token?
2. Explain temperature, top-k, and top-p (nucleus) sampling and their effects.
3. Greedy vs beam search vs sampling — tradeoffs.
4. What is a repetition penalty and why is it needed?
5. **How does speculative decoding work?** Why is it faster without changing the output distribution?
6. What is continuous batching and how does it help serving throughput?
7. **What is PagedAttention and why does vLLM use it?**

## Quantization & Precision
8. Explain floating-point representation (sign, exponent, mantissa) for FP32/FP16/BF16.
9. FP16 vs BF16 — why do we often prefer BF16 for training?
10. What is post-training quantization (PTQ)? INT8 tradeoffs.
11. GPTQ vs AWQ vs GGUF — what is each for and when do you use it?
12. What accuracy do you lose at 4-bit, and how do you measure it?

## Fine-Tuning (PEFT)
13. Pretraining vs fine-tuning — what does each change?
14. Why is full fine-tuning prohibitively expensive?
15. **Walk me through LoRA — the math, not just the concept.** What are rank and alpha?
16. What is QLoRA and how does it combine quantization + LoRA?
17. Prefix/prompt tuning — how do they differ from LoRA?
18. **When do you fine-tune vs prompt engineer vs RAG?**

## Advanced Architectures
19. What is a Mixture of Experts (MoE)? How does routing work and why does it scale cheaply?
20. What is knowledge distillation?
21. What is sparse attention and what does it buy you?

## Coding
22. Implement top-p sampling from scratch.
23. Quantize a model to 4-bit (GPTQ) and measure accuracy/latency.
24. Fine-tune a small model with LoRA (Unsloth) on a custom dataset.

## Judgment / Failure
25. Inference cost blew up in production — no batching/quantization. What went wrong?
26. "Fine-tune or few-shot prompt for this task?" — walk through the decision.
