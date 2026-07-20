# Week 4 — Interview Questions: Deep Learning Foundations

> Click a question (▸) to expand the answer.

---

## MLP & Backpropagation

<details>
<summary>1. What is a perceptron? What can't it do (XOR)?</summary>

A linear classifier: weighted sum + threshold. A single layer can only separate linearly separable data, so it can't learn XOR — you need a hidden layer (non-linear composition).
</details>

<details>
<summary>2. Compare Sigmoid, Tanh, ReLU, GELU, SiLU; why non-linearities?</summary>

Without non-linearity, stacked linear layers collapse to one linear map. Sigmoid/Tanh saturate (vanishing gradients); ReLU is cheap and non-saturating but can "die"; GELU/SiLU are smooth, self-gating, and standard in transformers.
</details>

<details>
<summary>3. Universal approximation theorem — what it does/doesn't say.</summary>

A wide enough one-hidden-layer net can approximate any continuous function. It does NOT say how wide, that training will find it, or that it generalizes — it's existence, not learnability.
</details>

<details>
<summary>4. Derive backprop through a two-layer network.</summary>

Forward: h=φ(W₁x), ŷ=W₂h. Loss L. Then ∂L/∂W₂ = δ₂hᵀ where δ₂=∂L/∂ŷ; backprop δ₁=(W₂ᵀδ₂)⊙φ'(z₁); ∂L/∂W₁=δ₁xᵀ. It's the chain rule applied layer by layer.
</details>

<details>
<summary>5. Causes of vanishing/exploding gradients; role of activation?</summary>

Repeated multiplication of Jacobian terms through depth shrinks or blows up gradients. Saturating activations (sigmoid) worsen vanishing; ReLU/GELU + good init + residuals + normalization mitigate it.
</details>

<details>
<summary>6. Why does weight init matter? Xavier vs He.</summary>

Bad init causes vanishing/exploding signals. Xavier keeps variance stable for tanh/sigmoid; He scales for ReLU (accounts for half the units being zero). Good init keeps activations/gradients well-scaled.
</details>

<details>
<summary>7. Why is ReLU popular despite dying ReLU?</summary>

Cheap, non-saturating for positive inputs, sparse activations, and strong gradients — training is fast and stable. Dying units are mitigated by Leaky/GELU variants and careful init/learning rate.
</details>

## Optimization (Deep)

<details>
<summary>8. Update rules for SGD+momentum, RMSProp, Adam.</summary>

Momentum: v=βv+∇; θ−=ηv. RMSProp: s=βs+(1−β)∇²; θ−=η∇/√(s+ε). Adam: combines both with bias-corrected first (m) and second (v) moments: θ−=η·m̂/(√v̂+ε).
</details>

<details>
<summary>9. Why does Adam outperform SGD on non-convex landscapes? Downsides?</summary>

Adaptive per-parameter rates + momentum navigate ravines and sparse gradients faster. Downsides: sometimes worse generalization and weight-decay coupling (use AdamW).
</details>

<details>
<summary>10. What is gradient clipping; when needed?</summary>

Cap the gradient norm to a threshold to prevent exploding updates — essential for RNNs and unstable transformer training.
</details>

<details>
<summary>11. Warmup and cosine annealing.</summary>

Warmup ramps the LR up initially to avoid early divergence; cosine annealing smoothly decays it for fine convergence.
</details>

## Training Mechanics

<details>
<summary>12. Signs/causes of overfitting; fixes?</summary>

Train loss ≪ val loss, val loss rising. Causes: too much capacity, too little/leaky data. Fixes: regularization, dropout, more/augmented data, early stopping, simpler model.
</details>

<details>
<summary>13. How does Dropout work; why regularize?</summary>

Randomly zeroes activations during training, preventing co-adaptation and approximating an ensemble of subnetworks. At inference it's off (weights scaled).
</details>

<details>
<summary>14. Derive BatchNorm; why does it help?</summary>

Normalize each feature over the batch (subtract mean, divide by std), then scale/shift with learned γ,β. It stabilizes activation distributions, allows higher learning rates, and smooths the loss landscape.
</details>

<details>
<summary>15. Why do transformers use LayerNorm not BatchNorm?</summary>

LayerNorm normalizes across features per token, independent of batch size and sequence length — crucial for variable-length sequences and small/streaming batches where batch statistics are unreliable.
</details>

<details>
<summary>16. Weight decay vs L2?</summary>

For plain SGD they're equivalent; for adaptive optimizers they differ, so AdamW decouples weight decay from the gradient update for correct regularization.
</details>

<details>
<summary>17. Early stopping; augmentation as regularization?</summary>

Early stopping halts when val loss stops improving (limits overfitting). Augmentation (crops, flips, noise) expands effective data and enforces invariances, acting as regularization.
</details>

## Scientific Thinking

<details>
<summary>18. Design an experiment to know if a change helped.</summary>

State a hypothesis, change one variable, hold others fixed (controls), pick a metric and a baseline, use enough seeds/data for significance, and watch confounders. Distinguish real signal from noise.
</details>

<details>
<summary>19. What is an ablation study; why run one?</summary>

Remove/replace one component at a time to measure its individual contribution — isolates what actually drives the result.
</details>

## Coding

<details>
<summary>20. Build an MLP from scratch; train MNIST >98%.</summary>

Implement linear layers, an activation, softmax + cross-entropy, forward/backward, and an optimizer; train with minibatches, BatchNorm/Dropout, and a decent LR schedule.
</details>

<details>
<summary>21. Implement BatchNorm and Dropout from scratch.</summary>

BatchNorm: normalize with batch mean/var in training (track running stats for inference), scale/shift with γ,β. Dropout: multiply by a Bernoulli mask in training, scale appropriately, disable at inference.
</details>

<details>
<summary>22. Ablation: with/without BatchNorm and Dropout.</summary>

Train four variants under identical settings and compare train/val curves and final accuracy to quantify each component's effect.
</details>

## Whiteboard / Judgment

<details>
<summary>23. L1 vs L2 regularization geometrically?</summary>

L2's circular constraint shrinks weights smoothly; L1's diamond has axis-aligned corners, driving some weights to exactly zero (sparsity/feature selection).
</details>

<details>
<summary>24. "BatchNorm or LayerNorm for this model?"</summary>

LayerNorm for transformers/sequence models and small/variable batches; BatchNorm for CNNs with large fixed batches. Tradeoff: BatchNorm's batch-statistic dependence breaks with tiny/variable batches; rejected it there.
</details>

<details>
<summary>25. Detect and fix silent gradient explosion in production.</summary>

Monitor gradient/activation norms and loss for NaNs/spikes; fix with gradient clipping, lower LR, better init/normalization, and check for bad data or mixed-precision overflow.
</details>
