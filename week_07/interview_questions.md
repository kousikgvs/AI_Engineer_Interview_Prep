# Week 7 — Interview Questions: Inference, Quantization & Fine-Tuning

> Click a question (▸) to expand the answer.

---

## Inference

<details>
<summary>1. How does autoregressive generation work token-by-token?</summary>

The model predicts a probability distribution over the next token, samples/selects one, appends it, and repeats — each step conditions on all previous tokens. The KV cache avoids recomputing past attention.
</details>

<details>
<summary>2. Temperature, top-k, top-p sampling and their effects?</summary>

Temperature scales logits (higher = more random, lower = more deterministic). Top-k keeps the k most likely tokens; top-p (nucleus) keeps the smallest set whose cumulative probability ≥ p. They trade off diversity vs coherence.
</details>

<details>
<summary>3. Greedy vs beam search vs sampling?</summary>

Greedy always takes the argmax (repetitive). Beam search keeps several high-probability sequences (good for translation, dull for open text). Sampling adds randomness for creative, varied output.
</details>

<details>
<summary>4. What is a repetition penalty; why needed?</summary>

It lowers the probability of already-generated tokens to stop loops/degeneration common in greedy/low-temperature decoding.
</details>

<details>
<summary>5. How does speculative decoding work?</summary>

A small fast "draft" model proposes several tokens; the big model verifies them in one parallel forward pass, accepting the longest correct prefix. It preserves the target distribution while cutting latency.
</details>

<details>
<summary>6. What is continuous batching?</summary>

Instead of waiting for a fixed batch, the server dynamically adds/removes requests each decoding step, keeping the GPU busy and maximizing throughput under mixed request lengths (vLLM).
</details>

<details>
<summary>7. What is PagedAttention; why does vLLM use it?</summary>

It manages the KV cache in fixed-size non-contiguous "pages" like OS virtual memory, eliminating fragmentation and enabling sharing across requests — so more sequences fit in GPU memory and throughput rises.
</details>

## Quantization & Precision

<details>
<summary>8. Explain FP32/FP16/BF16 (sign, exponent, mantissa).</summary>

Floats store sign + exponent (range) + mantissa (precision). FP16 has few exponent bits (small range, overflow risk); BF16 keeps FP32's exponent range with less mantissa — more stable for training.
</details>

<details>
<summary>9. FP16 vs BF16 — why prefer BF16 for training?</summary>

BF16's wider exponent range avoids the overflow/underflow that plagues FP16, so it trains stably without loss scaling while still halving memory.
</details>

<details>
<summary>10. What is PTQ; INT8 tradeoffs?</summary>

Post-training quantization converts a trained model's weights/activations to lower precision (e.g., INT8) without retraining — big memory/speed gains for a small, sometimes noticeable accuracy drop.
</details>

<details>
<summary>11. GPTQ vs AWQ vs GGUF — what and when?</summary>

GPTQ and AWQ are 4-bit weight-quantization methods (AWQ protects salient weights). GGUF is a file format for running quantized models on CPU/GPU via llama.cpp/Ollama. Use GGUF for local/CPU, GPTQ/AWQ for GPU serving.
</details>

<details>
<summary>12. Accuracy loss at 4-bit; how to measure?</summary>

Usually small but task-dependent. Measure by running your eval suite (perplexity + downstream accuracy) on the quantized vs full model and comparing latency/memory too.
</details>

## Fine-Tuning (PEFT)

<details>
<summary>13. Pretraining vs fine-tuning — what changes?</summary>

Pretraining learns general language from massive unlabeled data (expensive, once). Fine-tuning adapts that base to a task/style/format with far less labeled data.
</details>

<details>
<summary>14. Why is full fine-tuning too expensive?</summary>

It updates and stores all billions of parameters plus optimizer states — huge GPU memory, one full copy per task, and slow. PEFT avoids this.
</details>

<details>
<summary>15. Walk me through LoRA — the math.</summary>

Freeze W; learn a low-rank update ΔW = BA (B: d×r, A: r×d, r≪d), so the layer computes Wx + (α/r)BAx. Only A,B train — tiny param count, and you can merge or swap adapters.
</details>

<details>
<summary>16. What is QLoRA?</summary>

Fine-tune LoRA adapters on top of a 4-bit (NF4) quantized frozen base model, using paged optimizers — fits large models on a single GPU with minimal quality loss.
</details>

<details>
<summary>17. Prefix/prompt tuning vs LoRA?</summary>

They prepend learned virtual tokens/prefixes to the input instead of modifying weights — even fewer params but generally less expressive than LoRA.
</details>

<details>
<summary>18. When fine-tune vs prompt vs RAG?</summary>

Prompt/RAG for changing knowledge, quick iteration, and grounding. Fine-tune for fixed style/format/behavior or when prompts get too long/unreliable. Often combine RAG (facts) + light fine-tune (behavior).
</details>

## Advanced Architectures

<details>
<summary>19. What is a Mixture of Experts; why scale cheaply?</summary>

Many expert FFNs with a router that activates only a few per token, so parameter count grows without proportional compute — more capacity at similar FLOPs. Challenges: routing balance and memory.
</details>

<details>
<summary>20. What is knowledge distillation?</summary>

Train a small "student" to mimic a large "teacher's" outputs (soft probabilities), transferring capability into a cheaper model.
</details>

<details>
<summary>21. What is sparse attention; what does it buy?</summary>

Restrict attention to a subset of positions (local/strided/global) so cost grows sub-quadratically, enabling longer contexts at lower compute.
</details>

## Coding

<details>
<summary>22. Implement top-p sampling from scratch.</summary>

Softmax the logits, sort descending, take the smallest prefix whose cumulative probability ≥ p, renormalize that set, and sample from it.
</details>

<details>
<summary>23. Quantize to 4-bit (GPTQ); measure tradeoff.</summary>

Run GPTQ on the weights, then benchmark memory, tokens/sec, and eval accuracy vs the FP16 model to quantify the accuracy/latency tradeoff.
</details>

<details>
<summary>24. Fine-tune with LoRA (Unsloth).</summary>

Load a base model, attach LoRA adapters to attention/FFN projections, train on your dataset for a few epochs, then merge or serve the adapter.
</details>

## Judgment / Failure

<details>
<summary>25. Inference cost blew up — no batching/quantization?</summary>

Serving one request at a time in full precision wastes the GPU. Fix with continuous batching, quantization, KV-cache reuse, and right-sizing hardware; monitor tokens/sec and cost/query.
</details>

<details>
<summary>26. "Fine-tune or few-shot prompt for this task?"</summary>

**What I'd do:** start with few-shot prompting; fine-tune only if prompts are too long/costly or quality plateaus.
**Why:** prompting is faster/cheaper to iterate.
**Tradeoff:** fine-tuning gives consistency/lower latency but adds data/training/maintenance cost.
**Rejected:** immediate fine-tuning before proving the task needs it.
</details>
