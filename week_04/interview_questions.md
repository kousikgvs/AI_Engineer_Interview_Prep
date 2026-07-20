# Week 4 — Interview Questions: Deep Learning Foundations

> Sourced from DL-engineer and research screens (OpenAI/Anthropic-style fundamentals). Expect derivations.

---

## MLP & Backpropagation
1. What is a perceptron? What can't a single-layer perceptron do (XOR)?
2. Compare Sigmoid, Tanh, ReLU, GELU, SiLU — why do we need non-linearities at all?
3. State the universal approximation theorem — what does it say and what does it NOT guarantee?
4. **Derive backpropagation through a two-layer network.**
5. What causes vanishing and exploding gradients? How does activation choice affect this?
6. Why does weight initialization matter? Compare Xavier vs He initialization.
7. Why is ReLU popular despite the "dying ReLU" problem?

## Optimization (Deep)
8. Write the update rules for SGD with momentum, RMSProp, and Adam.
9. Why does Adam often outperform SGD on non-convex landscapes? What are its downsides?
10. What is gradient clipping and when do you need it?
11. Explain learning-rate warmup and cosine annealing.

## Training Mechanics
12. What are the signs and causes of overfitting? How do you fix it?
13. How does Dropout work, and why does it act like regularization / ensembling?
14. **Derive Batch Normalization. Why does it help training stability?**
15. **Why do transformers use LayerNorm instead of BatchNorm?**
16. What is weight decay, and how does it relate to L2 regularization?
17. What is early stopping? How does data augmentation act as regularization?

## Scientific Thinking
18. How do you design an experiment to know if a change actually helped (hypothesis, controls, metrics, confounders)?
19. What is an ablation study and why run one?

## Coding
20. Build an MLP from scratch (forward, backward, optimizer) and train on MNIST to >98%.
21. Implement BatchNorm and Dropout from scratch.
22. Run an ablation: with/without BatchNorm, with/without Dropout.

## Whiteboard / Judgment
23. What is the difference between L1 and L2 regularization geometrically?
24. "BatchNorm or LayerNorm for this model?" — defend it.
25. A production model had silent gradient explosion — how do you detect and fix it?
