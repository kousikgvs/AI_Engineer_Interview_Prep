# Week 7 — LLM Internals: Inference, Quantization & Fine-Tuning

**Block:** B — Transformers + LLM Internals  
**Goal:** Understand what happens after training — serving, efficiency, and adapting models.

---

## What You Will Learn

### Inference Deep Dive
- Autoregressive generation — how models generate one token at a time
- Temperature, top-k, top-p (nucleus) sampling — what they do and their effects
- Beam search vs greedy vs sampling
- Repetition penalty
- Speculative decoding — how a small draft model + a verifier model speeds things up
- Continuous batching — how vLLM serves many users at the same time
- PagedAttention — how vLLM manages KV cache memory

### Quantization & Precision
- Floating-point representation — how FP32/FP16/BF16 store numbers (sign, exponent, mantissa)
- Why quantization saves memory and compute
- FP32 → FP16 → BF16 — tradeoffs
- INT8 quantization — post-training quantization (PTQ)
- 4-bit quantization — GPTQ and AWQ
- Quantized model formats — GGUF, GPTQ, AWQ: what each is for and when to use it
- QLoRA — fine-tuning a quantized model
- Accuracy vs speed tradeoff analysis

### Parameter Efficient Fine-Tuning (PEFT)
- Pretraining vs fine-tuning — what each changes
- Why full fine-tuning is too expensive
- LoRA — Low-Rank Adaptation; the math behind it
- QLoRA — LoRA on a quantized model
- Prefix tuning and prompt tuning (for historical context)
- Hands-on PEFT with Unsloth (fast, memory-efficient fine-tuning)
- When to fine-tune vs when to use prompt engineering

### AI Infrastructure (First Look)
- vLLM — architecture and key innovations
- SGLang — structured generation serving
- TGI (Text Generation Inference)
- GPU memory hierarchy: HBM → L2 cache → SRAM
- Batching strategies and KV caching across requests

### Advanced Architectures & Compression
- Mixture of Experts (MoE) — sparse expert routing, why it scales capacity cheaply
- Knowledge distillation — training a small student model from a large teacher
- Sparse attention — reducing the quadratic attention cost
- How these combine with quantization for efficient serving

### Hugging Face Ecosystem
- Hugging Face Hub, datasets, and inference providers
- AutoModel, AutoTokenizer, AutoClasses
- `pipeline()` for quick inference; accessing instruction-tuned models (e.g., Gemma)
- Document Q&A pipelines with PyPDF

---

## Project
- Deploy an open-source LLM (Llama 3 or Mistral) locally using vLLM
- Implement top-p sampling from scratch
- Run 4-bit quantization with GPTQ and measure the accuracy/latency tradeoff
- Export a model to GGUF and run it locally (Ollama) — compare GGUF vs GPTQ vs AWQ
- Fine-tune a small model with LoRA (via Unsloth) on a custom dataset

---

## Papers to Read
- "LoRA: Low-Rank Adaptation of Large Language Models" — Hu et al. (2021)
- FlashAttention paper (concept level; full implementation in Week 8)

---

## Failure Friday
- Analyze: Inference cost blowup in production — a team that did not think about batching or quantization

---

## Interview Questions This Week Prepares You For

<details>
<summary>"How does speculative decoding work?"</summary>

A small draft model proposes several tokens; the large model verifies them in one parallel pass and accepts the longest correct prefix. It cuts latency while preserving the target model's output distribution.
</details>

<details>
<summary>"What is PagedAttention and why does vLLM use it?"</summary>

It manages the KV cache in fixed-size non-contiguous pages (like OS virtual memory), eliminating fragmentation and enabling sharing across requests — so more sequences fit in GPU memory and throughput rises.
</details>

<details>
<summary>"Walk me through LoRA — the math"</summary>

Freeze W and learn a low-rank update ΔW = BA (B: d×r, A: r×d, r≪d); the layer computes Wx + (α/r)BAx. Only A and B train, so the adapter is tiny and swappable/mergeable.
</details>

---

## Engineering Judgment Question

<details>
<summary><strong>"Fine-tune or use few-shot prompting for this task?"</strong></summary>

**What I'd do:** start with few-shot prompting; fine-tune only if prompts get too long/costly or quality plateaus.
**Why:** prompting iterates faster and cheaper.
**Tradeoff:** fine-tuning gives consistency/lower latency but adds data/training/maintenance cost.
**Rejected:** fine-tuning before proving the task needs it.
</details>
