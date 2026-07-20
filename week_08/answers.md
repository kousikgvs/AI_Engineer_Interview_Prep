# Week 8 — Answers with Code: GPU Architecture & AI Systems

> Hardware mental models + FLOP/memory math. Mostly conceptual with calc snippets.

---

## GPU Architecture

### 1. CPU vs GPU (easy)
CPU: few fast latency-optimized cores + branch prediction. GPU: thousands of simple throughput cores for parallel matmul.

### 2. CUDA execution model (detailed)
Threads → blocks (share fast shared memory) → grid. Hardware runs warps of 32 threads in lockstep (SIMT); branch divergence within a warp serializes → slow.

### 3. Memory hierarchy (detailed)
Registers (per-thread, fastest) → shared memory (per-block) → global/HBM (large, slow). Stage hot data in shared memory to cut global traffic.

### 4. Why memory-bound not compute (detailed)
FLOPs ≫ bandwidth. Elementwise/attention move lots of data per FLOP → bandwidth limits throughput. Hence fusion + Flash Attention.

### 5. HBM vs L2 vs SRAM (easy)
SRAM (KB–MB, fastest) < L2 (MBs) < HBM (tens of GB, high BW, higher latency).

### 6. TPU / systolic array (easy)
Grid of MAC units; data flows rhythmically, maximizing reuse for dense matmul.

---

## Performance Analysis

### 7. Roofline; attention bound? (detailed)
Roofline: attainable FLOPs = min(peak compute, bandwidth × arithmetic intensity). Attention is memory-bound → IO-aware Flash Attention helps.

### 8. FLOP counting (code)
```python
def matmul_flops(m, k, n): return 2 * m * k * n   # multiply + add
# attention scores QK^T: 2 * seq * seq * d ; times V: another 2*seq*seq*d
# FFN (4x): 2 * (2 * seq * d * 4d) dominates per layer
```

### 9. Arithmetic intensity (detailed)
AI = FLOPs / bytes moved. Low AI → memory-bound; raise it via fusion, larger tiles, reuse.

### 10. Why batch matmuls (easy)
Amortize weight loads over many tokens → higher AI, better GPU utilization.

---

## Distributed / Systems

### 11. Data vs tensor vs pipeline parallelism (detailed)
- Data: replicate model, split batch, all-reduce grads.
- Tensor: split individual layers/matrices across GPUs (intra-layer).
- Pipeline: split layers into stages across GPUs (inter-layer), micro-batches to fill the pipe.

### 12. ZeRO / FSDP (detailed)
Shard optimizer states, gradients, and params across GPUs so each holds a slice → train models larger than one GPU's memory.

### 13. All-reduce (easy)
Sum gradients across GPUs and broadcast the result (ring all-reduce is bandwidth-optimal).

### 14. Memory for training a model (code)
```python
# rough: params P
# weights: 2P (bf16) ; grads: 2P ; Adam states m,v: 8P (fp32) => ~12P bytes
def train_mem_gb(P): return 12 * P / 1e9
# plus activations (∝ batch × seq × layers)
```

### 15. Gradient checkpointing (detailed)
Recompute activations in backward instead of storing them → trade compute for memory (fit bigger models/batches).

### 16. Mixed precision (easy)
Compute in bf16/fp16, keep fp32 master weights + loss scaling → faster, stable.

---

## Coding / Estimation

### 17. Estimate KV cache size (code)
```python
def kv_cache_bytes(layers, seq, d_model, batch, bytes_=2):
    return 2 * layers * seq * d_model * batch * bytes_  # 2 = K and V
```

### 18. Estimate inference memory: weights + KV cache + activations.
### 19. Profile a kernel: check if bandwidth or compute bound via roofline/nsight.

---

## Judgment / Failure

### 20. OOM at long context: KV cache grows linearly with seq → use GQA, paged/quantized KV, shorter context, or offload.
### 21. Low GPU utilization: memory-bound or small batch → increase batch, fuse ops, use Flash Attention, continuous batching.
### 22. Multi-GPU no speedup: communication (all-reduce) bottleneck or poor overlap → gradient accumulation, better interconnect, overlap comm/compute.
