# Week 8 — Interview Questions: GPU Architecture & AI Systems

> Sourced from AI-infra / performance-engineering screens. This is where most candidates are weak — a differentiator.

---

## GPU Architecture
1. CPU vs GPU — what is the fundamental difference in design philosophy?
2. Explain the CUDA execution model: threads, blocks, grids, warps.
3. Describe the GPU memory hierarchy: global → shared → registers.
4. Why is memory bandwidth (not compute) usually the bottleneck?
5. What is HBM, L2 cache, SRAM — relative sizes, bandwidths, latencies?
6. What is a TPU / systolic array and how does it differ from a GPU?

## Performance Analysis
7. **What is the roofline model? Is attention compute-bound or memory-bound?**
8. How do you count FLOPs in attention, FFN, and linear layers?
9. What is arithmetic intensity (FLOPs/byte) and why does it matter?
10. Where does time actually go in a transformer forward pass?
11. How would you profile a GPU kernel?

## Parallelism
12. **Data vs tensor vs pipeline parallelism — explain each and when to use it.**
13. What is 3D parallelism?
14. What are the ZeRO stages (DeepSpeed) and what does each shard?
15. Explain all-reduce, all-gather, reduce-scatter and ring all-reduce.
16. What is gradient accumulation and when do you use it?
17. What is mixed-precision training (FP16 with BF16 master weights)?

## Kernels & Flash Attention
18. Why does Triton exist? What does it let you do?
19. **How does Flash Attention reduce memory complexity?** Explain online softmax + tiling.
20. Flash Attention v1 vs v2 — what improved?

## Coding
21. Write a shared-memory-optimized CUDA matmul kernel.
22. Implement a simplified Flash Attention (Triton) and benchmark vs naive attention.

## Judgment / Failure
23. A training run silently ran 10x slower — OOM fragmentation / bad parallelism. Diagnose it.
24. "Data parallelism or tensor parallelism for this model size?" — defend it.
