# Week 23 — Interview Questions: Deep Learning Extended — CV, GANs, VAEs & Diffusion

> Sourced from CV / multimodal / research-adjacent screens.

---

## Robust DL & CNNs
1. Explain a convolutional layer: kernels, stride, padding, channels.
2. **Why do CNNs use weight sharing and what does it buy you?**
3. What are pooling and receptive fields?
4. Walk from LeNet to ResNet — what problem did residual connections solve?
5. ReLU vs ELU vs GELU in practice.
6. How does transfer learning with a pretrained backbone work?

## Sequence & Multi-IO
7. When would you still use an RNN/LSTM/GRU over a transformer?
8. How do you design a multi-input / multi-output model? What is loss weighting?

## Generative Models
9. **Explain the difference between a GAN and a VAE.**
10. How does adversarial training work (generator vs discriminator)?
11. What is mode collapse and how do you stabilize GAN training?
12. What does the VAE latent space represent? Explain the reparameterization trick and the KL term.
13. What is an autoencoder used for (representation learning)?

## Diffusion & ViT / CLIP
14. **How does a diffusion model generate an image from noise** (forward/reverse process)?
15. What is inpainting / image-to-image?
16. What is a Vision Transformer (patches as tokens)?
17. **What does CLIP actually learn, and why is it zero-shot capable?**

## Multimodal
18. How do image and text embeddings align in a vision-language model?
19. What is Visual/Document Question Answering?

## Coding
20. Build a CNN classifier with augmentation + BatchNorm; implement a GAN and a VAE.

## Judgment
21. "GAN, VAE, or diffusion for this image-generation use case?" — defend it.
