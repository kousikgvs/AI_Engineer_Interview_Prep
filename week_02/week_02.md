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
- "Walk me through backpropagation from first principles"
- "Why does Adam converge faster than SGD?"
- "What is SVD and where would you use it in an ML system?"

---

## Engineering Judgment Question
**"Adam or SGD for this task? Defend your choice."**  
Write your answer covering: what you would do, why, what tradeoff you are making, and what alternative you rejected.
