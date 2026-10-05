---
title: Diffusion Models | Problems and Fixes
description: "Reading notes on diffusion models, organised as the problems the field hit and how they were fixed: the link to noise-conditioned score networks, poor log-likelihood fixed by a cosine variance schedule, unstable learned variances fixed by fixing them, and slow sampling fixed by DDIM and latent diffusion. Sources are listed at the end."
tags:
  - machine-learning
date: 2022-10-22
lastMod: 2026-10-06T00:00:00+08:00
---

Notes from reading Lilian Weng's "What are Diffusion Models?".[^weng] They skip the basics of the
forward and reverse processes, which her post explains well, and keep to the problems diffusion
models ran into and what fixed each one. Sources are collected at the bottom.

## Why diffusion models

Generative models usually trade **tractability** against **flexibility**. Tractable models, such
as a Gaussian or a Laplace distribution, can be evaluated analytically and fitted cheaply, but
they cannot describe the structure of rich data. Flexible models can fit arbitrary structure, but
evaluating, training or sampling from them is expensive. Diffusion models manage both.

The price is speed: a sample comes out of a long Markov chain of denoising steps, so generation is
slow in time and compute. Faster samplers exist, but sampling is still slower than with a GAN.

## Relation to score-based models

Diffusion models are closely related to noise-conditioned score networks (NCSN), which learn the
score, the gradient of the data's log density.

- **Estimating the score.** Denoising score matching[^dsm] and sliced score matching, which uses
  random projections,[^ssm] make the score learnable from data.
- **Stabilising training.** Adding a small Gaussian noise spreads the data distribution over the
  whole space \(\mathbb{R}^D\), which makes training the score estimator more stable.
- **Many noise levels.** Song and Ermon perturbed the data with noise at several levels and
  trained one noise-conditioned network to estimate the scores at all of them jointly.[^ncsn]

## Training

### Log-likelihood lagged behind other generative models

**Problem.** Diffusion models could not reach the log-likelihood of competing generative models.

**Fix.** Several training improvements, the best known being a cosine-based variance schedule for
\(\beta_t\).[^improved] The exact function matters less than its shape: it should drop almost
linearly in the middle of the process and change only subtly near \(t = 0\) and \(t = T\).

### Learning the reverse variance was unstable

**Problem.** Learning a diagonal variance \(\Sigma_\theta\) for the reverse process led to unstable
training and worse samples.

**Fix.** Ho et al. kept \(\beta_t\) as fixed constants instead of learning them and set
\(\Sigma_\theta(x_t, t) = \sigma_t^2 I\).[^ddpm]

## Sampling

**Problem.** Sampling from a DDPM follows the reverse Markov chain step by step, and \(T\) can be a
thousand steps or more. Song et al. put a number on it:[^ddim]

> For example, it takes around 20 hours to sample 50k images of size 32 × 32 from a DDPM, but less
> than a minute to do so from a GAN on an Nvidia 2080 Ti GPU.

Two fixes took different routes.

### Skip steps: DDIM

The denoising diffusion implicit model (DDIM) trains with any number of forward steps but samples
from only a subset of them.[^ddim] Compared with DDPM, it:

1. Produces better samples with far fewer steps.
2. Is **consistent**: generation is deterministic, so samples from the same latent share their
   high-level features.
3. Thanks to that consistency, interpolates meaningfully in the latent space.

### Diffuse less data: latent diffusion

The latent diffusion model (LDM) runs diffusion in a learned latent space instead of pixel space,
which makes both training and inference cheaper.[^ldm] It rests on an observation: most bits of an
image encode perceptual detail, while the semantic composition survives aggressive compression.

So LDM splits the work in two:

1. **Perceptual compression.** An autoencoder removes pixel-level redundancy. The encoder \(E\)
   maps an image \(x \in \mathbb{R}^{H \times W \times 3}\) to a smaller latent
   \(z = E(x) \in \mathbb{R}^{h \times w \times c}\), downsampling by \(f = H/h = W/w = 2^m\), and
   the decoder \(D\) reconstructs \(\hat{x} = D(z)\). To keep the latent's variance in check, the
   paper tries two regularisers: a small KL penalty towards a standard normal, as in a VAE, and a
   vector-quantisation layer absorbed into the decoder, as in VQ-VAE.
2. **Semantic generation.** Diffusion and denoising happen on \(z\). The denoiser is a
   time-conditioned U-Net with **cross-attention**, which lets it take conditioning of any kind,
   such as class labels, semantic maps or a blurred version of the image, by fusing the other
   modality's representation into the network.

[^weng]: [Lilian Weng: What are Diffusion Models?](https://lilianweng.github.io/posts/2021-07-11-diffusion-models/)
[^dsm]: [Vincent: A Connection Between Score Matching and Denoising Autoencoders](http://www.iro.umontreal.ca/~vincentp/Publications/smdae_techreport.pdf)
[^ssm]: [Song et al.: Sliced Score Matching: A Scalable Approach to Density and Score Estimation](https://arxiv.org/abs/1905.07088)
[^ncsn]: [Song and Ermon: Generative Modeling by Estimating Gradients of the Data Distribution](https://arxiv.org/abs/1907.05600)
[^improved]: [Nichol and Dhariwal: Improved Denoising Diffusion Probabilistic Models](https://arxiv.org/abs/2102.09672)
[^ddpm]: [Ho et al.: Denoising Diffusion Probabilistic Models](https://arxiv.org/abs/2006.11239)
[^ddim]: [Song et al.: Denoising Diffusion Implicit Models](https://arxiv.org/abs/2010.02502)
[^ldm]: [Rombach, Blattmann et al.: High-Resolution Image Synthesis with Latent Diffusion Models](https://arxiv.org/abs/2112.10752)
