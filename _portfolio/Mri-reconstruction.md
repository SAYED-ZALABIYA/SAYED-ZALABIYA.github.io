---
title: "Physics-Guided MRI Reconstruction Pipeline"
excerpt: "Diffusion-aware generator + unrolled ADMM refinement for accelerated MRI reconstruction, built and evaluated end-to-end on the fastMRI multi-coil dataset.<br/><img src='/images/admm_mri_reconstruction_flowchart.png'>"
collection: portfolio
---

**Stack:** 
* PyTorch
* fastMRI
*  multi-coil brain dataset
*  Google Colab (A100)
*  explicit MRI forward/adjoint operators

This is the engineering side of the [Physics-Informed MRI Reconstruction](/publications/) paper — the actual pipeline I built and iterated on before it became a co-authored write-up.

**What I built:**
- An ICDDGAN generator that takes zero-filled k-space and produces an initial reconstruction, conditioned on diffusion timestep embeddings and a latent vector.
- An 8-stage unrolled ADMM refinement loop, each stage doing hard k-space data consistency → a FiLM-conditioned learned proximal CNN → a dual-variable update.
- Explicit multi-coil forward/adjoint operators and an ACS-based coil sensitivity estimator, rather than treating the physics as a black box.
- A full ablation harness (zero-fill / generator-only / ADMM-only / full model) so every performance claim is checked against the alternative that removes it.

**Result:** 21.20 dB PSNR (full model) vs. −1.42 dB zero-fill baseline and −19.18 dB generator-only — the ablation that this project's write-up is actually built around.

**Honest scope:** trained on a reduced 50/30-file subset due to compute limits; still well below SOTA methods like VarNet. I consider this a working, interpretable prototype, not a leaderboard entry — see the [write-up](/publications/) for the full ablation table and limitations.

[Code on GitHub](https://github.com/SAYED-ZALABIYA/ICDDGAN-Unrolled-ADMM-MRI-Reconstruction) ·
