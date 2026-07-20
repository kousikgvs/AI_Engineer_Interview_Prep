# Week 2 — Interview Questions: Linear Algebra, Optimization & Calculus

> Sourced from ML-engineer and research-engineer screens. Be ready to derive on a whiteboard.

---

## Linear Algebra
1. What does matrix multiplication mean geometrically (as a linear transformation)?
2. Define rank, and what a low-rank matrix implies (ties to LoRA later).
3. What are eigenvalues and eigenvectors? What do they tell you about a transformation?
4. What is Singular Value Decomposition (SVD)? Where is it used in ML (embeddings, PCA, low-rank approx)?
5. Derive PCA — how does it relate to eigenvectors of the covariance matrix / SVD?
6. What is cosine similarity and why is it used for embedding retrieval instead of Euclidean distance?
7. When is a matrix invertible? What is the determinant intuitively?
8. What is the difference between a dot product and an outer product?

## Calculus & Autodiff
9. State the chain rule and explain why it is the engine of backpropagation.
10. What is a Jacobian? A Hessian? When do you need each?
11. What is a computational graph?
12. How does reverse-mode automatic differentiation (PyTorch autograd) work under the hood?
13. Difference between forward-mode and reverse-mode autodiff — when is each cheaper?

## Optimization
14. Derive gradient descent. What does the gradient represent?
15. Batch vs stochastic vs minibatch gradient descent — tradeoffs.
16. What are learning-rate warmup and cosine decay, and why use them?
17. Convex vs non-convex optimization — why do saddle points matter more than local minima in deep learning?
18. Why does Adam often converge faster than SGD? What are its update equations?
19. What is momentum and how does it help?
20. What causes exploding/vanishing gradients and how do you mitigate them?

## Applied / Decision-Making
21. Fermi: estimate the number of GPU hours to train GPT-2.
22. "Spend $5k on evals or more inference compute?" — how do you frame this?

## Coding
23. Implement gradient descent from scratch on a 2D loss surface.
24. Build a scalar-valued autograd engine (micrograd-style).
25. Visualize/compare SGD vs Adam vs RMSProp on the same loss surface.

## Whiteboard Classics
26. **Walk me through backpropagation from first principles.**
27. What is SVD and where would you use it in an ML system?
28. "Adam or SGD for this task?" — defend it.
