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
- "What is the roofline model? Is attention compute-bound or memory-bound?"
- "Walk me through Tensor Parallelism vs Pipeline Parallelism"
- "How does Flash Attention reduce memory complexity?"

---

## Engineering Judgment Question
**"Data parallelism or tensor parallelism for this model size?"**  
Write your answer covering: what you would do, why, what tradeoff you are making, and what alternative you rejected.
