# Week 2 — Interview Questions: Linear Algebra, Optimization & Calculus

> Click a question (▸) to expand the answer.

---

## Linear Algebra

<details>
<summary>1. What does matrix multiplication mean geometrically?</summary>

It applies a linear transformation — rotating, scaling, shearing, and projecting vectors. Columns of the matrix are where the basis vectors land.
</details>

<details>
<summary>2. Define rank; what does low rank imply (ties to LoRA)?</summary>

Rank = number of linearly independent rows/columns = dimension of the output space. Low rank means the transformation lives in a small subspace, so it's compressible — the basis for LoRA approximating weight updates with two small matrices.
</details>

<details>
<summary>3. What are eigenvalues/eigenvectors?</summary>

An eigenvector's direction is unchanged by the transformation; the eigenvalue is how much it's scaled (Av = λv). They reveal the "axes" and stretching factors of a linear map.
</details>

<details>
<summary>4. What is SVD and where is it used?</summary>

Any matrix A = UΣVᵀ: rotation, scaling by singular values, rotation. Used for low-rank approximation, PCA, embeddings compression, and pseudo-inverses.
</details>

<details>
<summary>5. Derive PCA — relation to eigenvectors/SVD.</summary>

PCA finds directions of maximum variance = top eigenvectors of the covariance matrix = top right singular vectors of the centered data. Projecting onto them gives the best low-rank reconstruction.
</details>

<details>
<summary>6. Cosine similarity — why for embeddings vs Euclidean?</summary>

Cosine measures angle (direction), ignoring magnitude, which suits embeddings where direction encodes meaning and vector length varies. Euclidean distance is dominated by magnitude differences.
</details>

<details>
<summary>7. When is a matrix invertible? Determinant intuition?</summary>

Invertible iff full rank / nonzero determinant. The determinant is the signed volume-scaling factor of the transformation; zero means it collapses space (loses information), so it's non-invertible.
</details>

<details>
<summary>8. Dot product vs outer product?</summary>

Dot product of two vectors → a scalar (similarity/projection). Outer product → a matrix (rank-1), each entry a product of components.
</details>

## Calculus & Autodiff

<details>
<summary>9. State the chain rule; why is it the engine of backprop?</summary>

d/dx f(g(x)) = f'(g(x))·g'(x). Backprop applies it layer by layer, multiplying local gradients from output back to inputs to get parameter gradients.
</details>

<details>
<summary>10. Jacobian vs Hessian — when do you need each?</summary>

Jacobian = matrix of first partial derivatives (vector-valued functions); Hessian = matrix of second derivatives (curvature). Jacobians for backprop through vector ops; Hessians for second-order optimization/convexity analysis.
</details>

<details>
<summary>11. What is a computational graph?</summary>

A DAG of operations where nodes are values and edges are dependencies. It lets autodiff record the forward pass and traverse backward to compute gradients.
</details>

<details>
<summary>12. How does reverse-mode autodiff (autograd) work?</summary>

The forward pass records operations and intermediate values; the backward pass propagates gradients from the scalar loss using the chain rule, reusing stored activations. Efficient when outputs ≪ inputs (one loss, many params).
</details>

<details>
<summary>13. Forward vs reverse-mode autodiff — when is each cheaper?</summary>

Forward-mode is cheap when inputs ≪ outputs; reverse-mode when outputs ≪ inputs. Deep learning has one scalar loss and millions of params, so reverse-mode wins.
</details>

## Optimization

<details>
<summary>14. Derive gradient descent; what does the gradient represent?</summary>

The gradient points in the direction of steepest increase; descent steps opposite it: θ ← θ − η∇L. It locally minimizes a first-order Taylor approximation of the loss.
</details>

<details>
<summary>15. Batch vs stochastic vs minibatch GD.</summary>

Batch uses all data (stable, slow, memory-heavy); stochastic uses one sample (noisy, fast); minibatch (32–1024) balances stable gradients with hardware efficiency — the default.
</details>

<details>
<summary>16. Learning-rate warmup and cosine decay — why?</summary>

Warmup starts small to avoid early instability with large/random weights; cosine decay gradually lowers the rate for fine convergence. Together they stabilize and improve final loss.
</details>

<details>
<summary>17. Convex vs non-convex; why saddle points matter more than local minima?</summary>

Convex problems have one global minimum. In high-dimensional deep nets, most critical points are saddle points (some directions up, some down), not bad local minima — so escaping saddles (momentum, noise) matters most.
</details>

<details>
<summary>18. Why does Adam often converge faster than SGD? Downsides?</summary>

Adam adapts per-parameter learning rates using first/second moment estimates (momentum + RMSProp), handling sparse/ill-scaled gradients well. Downsides: can generalize slightly worse and needs weight-decay care (AdamW).
</details>

<details>
<summary>19. What is momentum and how does it help?</summary>

It accumulates a moving average of gradients, smoothing noise and accelerating along consistent directions while damping oscillations across ravines.
</details>

<details>
<summary>20. Causes of exploding/vanishing gradients; mitigations?</summary>

Repeated multiplication of large/small Jacobian terms through depth. Mitigate with good init (He/Xavier), residual connections, normalization, gradient clipping, and non-saturating activations (ReLU/GELU).
</details>

## Applied / Decision-Making

<details>
<summary>21. Fermi: GPU hours to train GPT-2?</summary>

Use compute ≈ 6·params·tokens FLOPs ÷ (GPU FLOPs × utilization). Show the decomposition; the exact figure matters less than the reasoning.
</details>

<details>
<summary>22. "$5k on evals or more inference compute?"</summary>

Frame by expected value: evals de-risk quality and prevent costly regressions/incidents (compounding value), while extra compute gives marginal latency/throughput. Usually invest in evals first unless you're capacity-bound.
</details>

## Coding

<details>
<summary>23. Implement gradient descent on a 2D loss surface.</summary>

Compute the analytic (or numeric) gradient at the current point, step opposite it scaled by η, and iterate; log the trajectory to visualize convergence.
</details>

<details>
<summary>24. Build a scalar autograd engine (micrograd).</summary>

Wrap each value in an object storing its data and a `_backward` closure; overload `+`, `*`, etc. to build a graph; topologically sort and call `_backward` from the output to accumulate `.grad`.
</details>

<details>
<summary>25. Compare SGD vs Adam vs RMSProp on one surface.</summary>

Run each optimizer from the same start on the same loss and plot the paths; Adam/RMSProp adapt step sizes and usually navigate ravines faster than plain SGD.
</details>

## Whiteboard Classics

<details>
<summary>26. Walk me through backpropagation from first principles.</summary>

Forward pass computes outputs and loss; backward pass applies the chain rule layer by layer, multiplying local derivatives to get ∂L/∂w for every weight, then gradient descent updates them. Store activations during forward to reuse in backward.
</details>

<details>
<summary>27. What is SVD and where would you use it in an ML system?</summary>

A = UΣVᵀ. Use it for dimensionality reduction (PCA), low-rank weight/embedding compression, denoising, recommender factorization, and computing pseudo-inverses.
</details>

<details>
<summary>28. "Adam or SGD for this task?"</summary>

Adam for fast convergence, sparse gradients, or quick prototyping; SGD+momentum (with schedule) when you want best generalization on large vision/LM training. Tradeoff: Adam is easier to tune but can generalize slightly worse; rejected SGD when tuning time is scarce.
</details>
