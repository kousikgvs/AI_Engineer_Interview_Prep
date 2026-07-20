# Week 8 — GPU Architecture & AI Systems

**Block:** C — Systems + Backend + Data Engineering  
**Goal:** Understand the hardware your models run on. This is where 99% of bootcamps fail.

---

## What You Will Learn

### GPU Architecture
- CPU vs GPU — different philosophies about parallelism
- CUDA programming model — threads, blocks, grids, warps
- GPU memory hierarchy: global memory → shared memory → registers
- Memory bandwidth is the bottleneck, not raw compute
- TPU architecture — systolic arrays
- Memory hierarchy: HBM, L2 cache, SRAM — sizes, bandwidths, and latencies

### Performance Analysis
- Roofline model — is an operation compute-bound or memory-bound?
- FLOPs — how to count operations in attention, feed-forward, and linear layers
- Arithmetic intensity — FLOPs per byte
- How to profile GPU kernels
- Where time actually goes in a transformer forward pass

### Parallelism Strategies
- Data Parallelism (DP) — replicate the model, split the batch
- Tensor Parallelism (TP) — split individual operations across GPUs
- Pipeline Parallelism (PP) — split layers across GPUs
- Sequence Parallelism
- 3D Parallelism — combining all three
- ZeRO stages (DeepSpeed) — sharding optimizer state, gradients, and parameters

### Distributed Training
- All-reduce, all-gather, reduce-scatter operations
- NCCL — the GPU communication library
- Ring all-reduce algorithm
- Gradient accumulation
- Mixed precision training — FP16 with BF16 master weights

### Triton and Flash Attention
- Why Triton exists — writing GPU kernels in Python
- Flash Attention: online softmax trick, tiling in SRAM, IO complexity analysis
- Flash Attention vs Flash Attention 2 improvements

---

## Project
- Write CUDA kernels for matrix multiplication — optimize with shared memory
- Implement Flash Attention (simplified Triton version)
- Profile transformer training: identify the top 3 bottlenecks
- Benchmark throughput improvements from each optimization

---

## Paper to Read
- "FlashAttention: Fast and Memory-Efficient Exact Attention with IO-Awareness" — Dao et al.

---

## Failure Friday
- Analyze: A training run that silently ran 10x slower than expected — OOM fragmentation, suboptimal parallelism

---

## Interview Questions This Week Prepares You For

<details>
<summary>"What is the roofline model? Is attention compute- or memory-bound?"</summary>

The roofline caps achievable FLOPs by either compute or memory bandwidth vs arithmetic intensity. Attention is largely memory-bound (much data movement per FLOP), which is why IO-aware Flash Attention helps.
</details>

<details>
<summary>"Walk me through Tensor Parallelism vs Pipeline Parallelism"</summary>

Tensor parallelism splits individual matmuls across GPUs (needs fast interconnect, low latency). Pipeline parallelism splits layers across GPUs and streams microbatches (has bubble overhead). They're often combined for huge models.
</details>

<details>
<summary>"How does Flash Attention reduce memory complexity?"</summary>

It tiles Q/K/V in SRAM and computes softmax incrementally (online softmax), never writing the full N×N matrix to HBM — memory drops from O(N²) to O(N) and it runs faster.
</details>

---

## Engineering Judgment Question

<details>
<summary><strong>"Data parallelism or tensor parallelism for this model size?"</strong></summary>

**What I'd do:** data parallelism while the model fits on one GPU; add tensor (and pipeline) parallelism once it doesn't.
**Why:** DP is simplest and scales throughput.
**Tradeoff:** TP adds heavy inter-GPU communication.
**Rejected:** TP for models that fit in a single GPU.
</details>
