# Week 2 — Linear Algebra, Optimization & Calculus for ML

**Block:** A — Math + ML Foundations  
**Goal:** Build the math tools behind gradient descent and backpropagation.

---

## What You Will Learn

### Linear Algebra
- Vectors, matrices, and tensors
- Matrix multiplication and what it means geometrically
- Transpose, inverse, rank, and determinant
- Eigenvalues and eigenvectors
- Singular Value Decomposition (SVD) — why it matters for embeddings
- PCA (Principal Component Analysis) from scratch
- Dot products and cosine similarity — the foundation of embedding search

### Calculus
- Derivatives and partial derivatives
- The chain rule — the engine that makes backpropagation work
- Jacobians and Hessians
- Computational graphs
- Automatic differentiation — how PyTorch's autograd works under the hood

### Optimization
- Gradient descent — derivation and intuition
- Stochastic Gradient Descent (SGD) and minibatch gradient descent
- Learning rate schedules — warmup and cosine decay
- Convexity, local minima vs global minima
- Saddle points — why they are common in deep learning

### Decision Making Under Uncertainty
- Fermi problems: estimate how many GPU hours it takes to train GPT-2
- Cost-benefit framing: "Should we spend $5k on evals or more compute?"
- Expected value applied to experiment prioritization

---

## Project
- Implement gradient descent from scratch on a 2D loss surface
- Build a basic autograd engine (scalar-valued, like micrograd)
- Visualize optimization paths: SGD vs Adam vs RMSProp on the same loss surface

---

## Paper to Read
- Karpathy's "micrograd" walkthrough + source code

---

## Failure Friday
- Analyze: A real training run that diverged because of a bad learning rate — what happened, and what should have been caught?

---

## Interview Questions This Week Prepares You For

<details>
<summary>"Walk me through backpropagation from first principles"</summary>

Forward pass computes outputs and loss; backward pass applies the chain rule layer by layer, multiplying local derivatives to get ∂L/∂w for every weight, then gradient descent updates them. Activations stored in the forward pass are reused in the backward pass.
</details>

<details>
<summary>"Why does Adam converge faster than SGD?"</summary>

Adam adapts a per-parameter learning rate using running estimates of the gradient's first and second moments (momentum + RMSProp), so it handles sparse and ill-scaled gradients better. Downside: it can generalize slightly worse (use AdamW).
</details>

<details>
<summary>"What is SVD and where would you use it in an ML system?"</summary>

A = UΣVᵀ factorizes any matrix into rotation·scaling·rotation. Uses: PCA/dimensionality reduction, low-rank weight/embedding compression, recommender factorization, denoising, pseudo-inverses.
</details>

---

## Engineering Judgment Question

<details>
<summary><strong>"Adam or SGD for this task?"</strong></summary>

**What I'd do:** Adam for fast convergence, sparse gradients, or prototyping; SGD+momentum with a schedule for best generalization on large training runs.
**Why:** Adam's adaptive rates speed early progress; well-tuned SGD often finds flatter, better-generalizing minima.
**Tradeoff:** ease of tuning vs final generalization.
**Rejected:** SGD when tuning time/compute is scarce.
</details>
