# Week 4 — Deep Learning Foundations

**Block:** A — Math + ML Foundations  
**Goal:** Understand neural networks from scratch. Build every part manually before touching PyTorch.

---

## What You Will Learn

### Multilayer Perceptron (MLP)
- The perceptron — the simplest neural network
- Activation functions: Sigmoid, Tanh, ReLU, GELU, SiLU — what they are and why we need them
- Universal approximation theorem — what it says and what it does NOT say
- Forward pass as a series of matrix multiplications
- Backpropagation — full derivation through a two-layer network
- Vanishing and exploding gradients — why deep networks are hard to train
- Weight initialization — Xavier and He initialization, why it matters

### Optimization (Deep)
- SGD with momentum
- RMSProp — adaptive learning rates
- Adam — combining momentum and adaptive rates (full derivation)
- Learning rate warmup and cosine annealing
- Gradient clipping
- Why Adam often beats SGD on non-convex problems

### Training Mechanics
- Overfitting — signs, symptoms, causes
- Regularization: Dropout, Weight Decay, L1/L2
- Batch Normalization — what it does and why it helps training stability
- Layer Normalization — why transformers use this instead of BatchNorm
- Early stopping
- Data augmentation as a form of regularization

### Scientific Thinking
- How to design a proper experiment: hypothesis, variables, controls, metrics
- "How do I know this actually works?" vs "Twitter said it works"
- Ablation studies — changing one thing at a time

---

## Project
- Build a full MLP framework from scratch (forward pass, backward pass, optimizers)
- Train on MNIST — target above 98% accuracy
- Implement BatchNorm and Dropout from scratch
- Ablation study: compare training with and without BatchNorm, with and without Dropout

---

## Paper to Read
- LeCun et al. (1998) "Gradient-Based Learning Applied to Document Recognition"

---

## Failure Friday
- Analyze: A case of silent gradient explosion in production — how do you detect it? How do you fix it?

---

## Interview Questions This Week Prepares You For

<details>
<summary>"Derive backpropagation through a two-layer network"</summary>

Forward: h=φ(W₁x), ŷ=W₂h, loss L. Backward: δ₂=∂L/∂ŷ, ∂L/∂W₂=δ₂hᵀ; δ₁=(W₂ᵀδ₂)⊙φ'(z₁), ∂L/∂W₁=δ₁xᵀ — the chain rule layer by layer.
</details>

<details>
<summary>"Why does BatchNorm help? Why do transformers prefer LayerNorm?"</summary>

BatchNorm stabilizes activation distributions and smooths the loss surface, allowing higher LRs. Transformers use LayerNorm because it normalizes per token (independent of batch size and variable sequence length), unlike BatchNorm's unreliable batch statistics.
</details>

<details>
<summary>"What is the difference between L1 and L2 regularization geometrically?"</summary>

L2's circular constraint shrinks weights smoothly; L1's diamond has axis-aligned corners, pushing some weights to exactly zero (sparsity/feature selection).
</details>

---

## Engineering Judgment Question

<details>
<summary><strong>"BatchNorm or LayerNorm?"</strong></summary>

**What I'd do:** LayerNorm for transformers/sequence models and small/variable batches; BatchNorm for CNNs with large fixed batches.
**Why:** LayerNorm is batch-size independent; BatchNorm exploits batch statistics that help vision.
**Tradeoff:** BatchNorm breaks with tiny/variable batches.
**Rejected:** BatchNorm for sequence/streaming workloads.
</details>
