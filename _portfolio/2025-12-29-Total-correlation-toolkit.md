---
title: "Total Correlation Dynamics Toolkit"
excerpt: "A training-and-measurement harness that tracks Gaussian Total Correlation alongside accuracy, epoch by epoch, across MLP and RNN architectures — the codebase behind the decorrelation study.<br/><img src='/images/Figure_1.png'>"
collection: portfolio
---

**Stack:** 
*  PyTorch
*  NumPy (covariance-based TC estimation)
*  Matplotlib
*  multi-seed experiment runner

This is the engineering side of the [Task Optimization Drives Statistical Decorrelation](/publications/) preprint — the harness I built to actually run and measure the experiment, not just the write-up of what it found.

**What I built:**
- A continuous noisy-XOR data generator (N=1024) — a controllable, non-linearly-separable benchmark task for both architectures.
- Parallel training loops for a Tanh MLP and a Tanh RNN, with hidden-state activations collected on every evaluation pass.
- A **Gaussian Total Correlation estimator** computed directly from the hidden-unit covariance matrix, with a numerically stabilized log-determinant (diagonal regularization at ε = 10⁻⁶) so it doesn't blow up on near-singular covariance matrices.
- A layer-wise breakdown mode, so TC can be tracked per-layer, not just for the network as a whole.
- A multi-seed runner that repeats every configuration across random seeds and reports mean ± standard deviation, rather than a single run's curve.

**Result:** this is the tool that produced the headline numbers — MLP TC falling from 35.3 to 16.0 nats as accuracy rose from ~55% to ~88% (shown above), and the equivalent RNN trajectory (38.2 → 20.4 nats) with its higher baseline from temporal feedback.

**Why it's here, not just in the paper:** the TC estimator and the multi-seed harness are reusable — anyone wanting to check whether *their* network decorrelates during training can point this at a different architecture or task without rebuilding the measurement machinery from scratch.

[Code on GitHub](https://github.com/SAYED-ZALABIYA/integration-dynamics-neural-networks) · [Full paper](/publications/)
