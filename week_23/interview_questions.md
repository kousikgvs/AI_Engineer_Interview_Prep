# Week 23 — Interview Questions: Deep Learning Extended — CV, GANs, VAEs & Diffusion

> Click a question (▸) to expand the answer.

---

## Robust DL & CNNs

<details>
<summary>1. Explain a convolutional layer.</summary>

A small learnable kernel slides over the input computing local weighted sums, producing feature maps. Stride controls step size, padding controls output size, and each kernel detects a pattern (edges, textures).
</details>

<details>
<summary>2. Why do CNNs use weight sharing; what does it buy?</summary>

The same kernel applies across all positions, giving translation invariance and far fewer parameters than a dense layer — better generalization and efficiency on images.
</details>

<details>
<summary>3. What are pooling and receptive fields?</summary>

Pooling (max/avg) downsamples feature maps for invariance and efficiency. The receptive field is the input region influencing one output unit; it grows with depth, letting deep layers see larger context.
</details>

<details>
<summary>4. LeNet → ResNet — what did residuals solve?</summary>

Deeper plain nets degraded due to vanishing gradients/optimization difficulty. ResNet's skip connections let layers learn residuals, enabling very deep networks to train.
</details>

<details>
<summary>5. ReLU vs ELU vs GELU in practice?</summary>

ReLU: simple, fast, can die. ELU: negative saturation, smoother. GELU: smooth probabilistic gating, standard in transformers. Choose by architecture/empirics.
</details>

<details>
<summary>6. How does transfer learning with a pretrained backbone work?</summary>

Take a network pretrained on large data (ImageNet), replace/fine-tune the head for your task, and optionally unfreeze deeper layers — reusing learned features to succeed with little data.
</details>

## Sequence & Multi-IO

<details>
<summary>7. When still use RNN/LSTM/GRU over a transformer?</summary>

For small data, streaming/low-latency sequential inference, tiny models on edge devices, or simple time-series where transformers are overkill.
</details>

<details>
<summary>8. Multi-input/output models; loss weighting?</summary>

Fuse multiple input streams (image + metadata) and produce multiple outputs with separate heads. Loss weighting balances the tasks so one doesn't dominate training.
</details>

## Generative Models

<details>
<summary>9. GAN vs VAE — the difference.</summary>

A GAN trains a generator against a discriminator (adversarial) for sharp samples but unstable training. A VAE learns an encoder/decoder with a probabilistic latent space (stable, principled) but blurrier samples.
</details>

<details>
<summary>10. How does adversarial training work?</summary>

The discriminator learns to tell real from generated; the generator learns to fool it. They improve together in a minimax game until generated samples look real.
</details>

<details>
<summary>11. What is mode collapse; how stabilize?</summary>

The generator produces limited variety (one or few outputs). Stabilize with techniques like Wasserstein loss, gradient penalty, minibatch discrimination, spectral normalization, and careful LR/architecture.
</details>

<details>
<summary>12. VAE latent space; reparameterization trick; KL term?</summary>

The encoder outputs a distribution; you sample z = μ + σ⊙ε (reparameterization) so gradients flow through the sampling. The KL term regularizes the latent toward a prior (standard normal), enabling smooth sampling.
</details>

<details>
<summary>13. What is an autoencoder used for?</summary>

Learning compressed representations by reconstructing the input through a bottleneck — for dimensionality reduction, denoising, anomaly detection, and pretraining.
</details>

## Diffusion & ViT / CLIP

<details>
<summary>14. How does a diffusion model generate an image from noise?</summary>

Training gradually adds noise to images; the model learns to predict/remove that noise. Generation starts from pure noise and iteratively denoises over many steps into a coherent image.
</details>

<details>
<summary>15. What is inpainting / image-to-image?</summary>

Inpainting fills masked regions consistently with surroundings; image-to-image transforms an input image (style/edit) by conditioning the diffusion process on it.
</details>

<details>
<summary>16. What is a Vision Transformer (patches as tokens)?</summary>

ViT splits an image into fixed patches, embeds each as a token, adds positional encodings, and applies a standard transformer — treating vision as sequence modeling.
</details>

<details>
<summary>17. What does CLIP learn; why zero-shot?</summary>

CLIP trains image and text encoders with contrastive loss so matching image-text pairs are close in a shared space. You classify zero-shot by comparing an image to text prompts of candidate labels — no task-specific training.
</details>

## Multimodal

<details>
<summary>18. How do image and text embeddings align?</summary>

Via contrastive training on paired data, both modalities map into one shared embedding space where semantically matching items are near each other.
</details>

<details>
<summary>19. What is Visual/Document Question Answering?</summary>

Answering questions about an image (VQA) or a document's content/layout (DocVQA) by jointly reasoning over visual and text features.
</details>

## Coding

<details>
<summary>20. Build a CNN classifier; implement a GAN and a VAE.</summary>

CNN: conv-BN-ReLU-pool blocks + a classifier head with augmentation. GAN: generator + discriminator trained adversarially. VAE: encoder → latent (μ,σ) → decoder with reconstruction + KL loss.
</details>

## Judgment

<details>
<summary>21. "GAN, VAE, or diffusion for this image-generation use case?"</summary>

**What I'd do:** diffusion for highest-quality, controllable image generation; VAE for a smooth latent/representation or fast sampling; GAN for fast generation when you can manage training instability.
**Why:** diffusion leads quality; VAEs are stable/structured.
**Tradeoff:** diffusion is slow at inference (many steps).
**Rejected:** GANs when training stability/mode collapse is a risk and quality matters most.
</details>
