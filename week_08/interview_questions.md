# Week 8 — Interview Questions: GPU Architecture & AI Systems

> Click a question (▸) to expand the answer.

---

## GPU Architecture

<details>
<summary>1. CPU vs GPU design philosophy?</summary>

CPUs have few powerful cores optimized for latency and branching; GPUs have thousands of simple cores optimized for throughput on parallel, regular workloads like matrix math.
</details>

<details>
<summary>2. CUDA execution model: threads, blocks, grids, warps?</summary>

Threads run the kernel; they're grouped into blocks (share fast shared memory), blocks form a grid. Hardware executes threads in warps of 32 in lockstep (SIMT); divergence within a warp hurts performance.
</details>

<details>
<summary>3. GPU memory hierarchy: global → shared → registers?</summary>

Registers (per-thread, fastest), shared memory (per-block, fast, programmer-managed), global/HBM (large, slow). Good kernels stage data through shared memory to cut global-memory traffic.
</details>

<details>
<summary>4. Why is memory bandwidth the bottleneck, not compute?</summary>

Modern GPUs have far more FLOPs than memory bandwidth, so many ops (attention, elementwise) are limited by how fast data moves, not by arithmetic — the reason for fusion and Flash Attention.
</details>

<details>
<summary>5. HBM vs L2 vs SRAM — sizes/bandwidths/latencies?</summary>

SRAM (on-chip, tens of KB–MB, fastest, lowest latency), L2 (MBs), HBM (tens of GB, high bandwidth but ~higher latency). Keeping hot data in SRAM is key to performance.
</details>

<details>
<summary>6. What is a TPU / systolic array?</summary>

A grid of multiply-accumulate units where data flows rhythmically through the array, maximizing reuse for dense matmuls — very efficient for the matrix ops in deep learning.
</details>

## Performance Analysis

<details>
<summary>7. Roofline model; is attention compute- or memory-bound?</summary>

The roofline plots achievable FLOPs vs arithmetic intensity, capped by compute or bandwidth. Attention is largely memory-bound (lots of data movement per FLOP), which is why Flash Attention (IO-aware) helps.
</details>

<details>
<summary>8. Count FLOPs in attention/FFN/linear layers.</summary>

A matmul of (m×k)·(k×n) ≈ 2mkn FLOPs. Attention scores/values scale with seq²·d; the FFN (4× expansion) dominates per-layer compute. Sum across layers.
</details>

<details>
<summary>9. Arithmetic intensity; why does it matter?</summary>

FLOPs per byte moved. Low intensity = memory-bound (optimize data movement); high intensity = compute-bound (optimize FLOPs). It tells you which side of the roofline you're on.
</details>

<details>
<summary>10. Where does time go in a transformer forward pass?</summary>

Mostly matmuls (attention projections + FFN) and memory-bound ops (softmax, normalization, elementwise). Profiling usually shows FFN and attention data movement dominating.
</details>

<details>
<summary>11. How would you profile a GPU kernel?</summary>

Use Nsight/torch profiler to measure kernel time, occupancy, memory throughput, and whether kernels are compute- or memory-bound; find the top few kernels and optimize them.
</details>

## Parallelism

<details>
<summary>12. Data vs tensor vs pipeline parallelism?</summary>

Data: replicate the model, split the batch, all-reduce gradients. Tensor: split individual matmuls across GPUs (needs fast interconnect). Pipeline: split layers across GPUs and stream microbatches. Combine them for huge models.
</details>

<details>
<summary>13. What is 3D parallelism?</summary>

Combining data + tensor + pipeline parallelism to train models too big for any single axis — used for the largest LLMs.
</details>

<details>
<summary>14. ZeRO stages — what does each shard?</summary>

Stage 1 shards optimizer states, stage 2 also gradients, stage 3 also parameters — progressively cutting per-GPU memory at the cost of more communication.
</details>

<details>
<summary>15. All-reduce, all-gather, reduce-scatter, ring all-reduce?</summary>

All-reduce sums values across GPUs and shares the result; all-gather concatenates; reduce-scatter reduces then splits. Ring all-reduce passes chunks around a ring for bandwidth-optimal syncing.
</details>

<details>
<summary>16. What is gradient accumulation; when use it?</summary>

Sum gradients over several microbatches before stepping, simulating a large batch when memory is limited.
</details>

<details>
<summary>17. Mixed-precision training (FP16 + BF16 master weights)?</summary>

Compute in low precision for speed/memory while keeping higher-precision master weights and loss scaling for numerical stability.
</details>

## Kernels & Flash Attention

<details>
<summary>18. Why does Triton exist?</summary>

It lets you write fast, fused GPU kernels in Python without low-level CUDA, giving near-hand-tuned performance with far less effort.
</details>

<details>
<summary>19. How does Flash Attention reduce memory complexity?</summary>

It tiles Q/K/V into SRAM and computes softmax incrementally (online softmax), never materializing the full N×N attention matrix in HBM — memory drops from O(N²) to O(N) and it runs faster.
</details>

<details>
<summary>20. Flash Attention v1 vs v2?</summary>

v2 improves work partitioning and parallelism (better occupancy, fewer non-matmul ops), giving significantly higher throughput than v1.
</details>

## Coding

<details>
<summary>21. Shared-memory-optimized CUDA matmul.</summary>

Load tiles of A and B into shared memory, have each thread compute a sub-block reusing those tiles, and loop over tiles — cutting global-memory reads dramatically.
</details>

<details>
<summary>22. Implement simplified Flash Attention; benchmark.</summary>

Tile Q/K/V, maintain running max and sum for online softmax, accumulate the output per tile, and compare speed/memory to naive attention.
</details>

## Judgment / Failure

<details>
<summary>23. Training ran 10x slower — diagnose.</summary>

Likely OOM-driven memory fragmentation/recompute, bad parallelism, small batch, or CPU/data-loading bottleneck. Profile kernels and data pipeline; fix parallelism, batch size, and memory allocation.
</details>

<details>
<summary>24. "Data or tensor parallelism for this model size?"</summary>

**What I'd do:** data parallelism if the model fits on one GPU; tensor (and pipeline) parallelism once it doesn't.
**Why:** DP is simplest and scales throughput; TP splits big layers but needs fast interconnect.
**Tradeoff:** TP adds communication overhead.
**Rejected:** TP for models that fit in one GPU's memory.
</details>
