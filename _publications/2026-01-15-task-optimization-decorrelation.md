---
title: "Task Optimization Drives Statistical Decorrelation: An Empirical Study of Integration Dynamics in Feed-Forward and Recurrent Neural Networks"
collection: publications
permalink: /publication/2026-01-15-task-optimization-decorrelation
excerpt: 'An empirical study tracking Gaussian Total Correlation (a statistical proxy for IIT''s Φ) during training of MLPs and RNNs, finding a consistent inverse relationship between task accuracy and internal statistical integration.'
date: 2026-01-15
venue: 'Zenodo (Preprint)'
paperurl: 'http://SAYED-ZALABIYA.github.io/files/task-optimization-decorrelation.pdf'
citation: 'Mohammed, E. A. (2026). &quot;Task Optimization Drives Statistical Decorrelation: An Empirical Study of Integration Dynamics in Feed-Forward and Recurrent Neural Networks.&quot; <i>Zenodo Preprint</i>.'
---

Integrated Information Theory (IIT) posits that consciousness relates to a system's capacity for information integration (Φ). Modern deep learning, by contrast, optimizes for task performance — often through mechanisms that promote feature disentanglement. This paper asks directly: does task optimization support or suppress statistical integration?

**Method.** MLP and RNN classifiers were trained on a continuous noisy-XOR task (N = 1024 samples) for 40 epochs across multiple random seeds. At each epoch, Gaussian Total Correlation (TC) — a tractable statistical proxy for multivariate dependency among hidden units — was estimated from the covariance structure of hidden activations, alongside classification accuracy.

**Key findings.**
- **MLP:** as accuracy rose from ~55% to ~88%, TC fell from 35.3 to 16.0 nats — a reduction of over 50%, concentrated most strongly in deeper layers.
- **RNN:** maintained a consistently higher baseline TC (38.2 → 20.4 nats) due to temporal feedback across hidden states, yet exhibited the same overall decorrelation trend under optimization.
- **Correlation strength:** accuracy and TC were strongly negatively correlated in both architectures (MLP r = −0.87, RNN r = −0.92).

**Interpretation.** These results are consistent with the Information Bottleneck view of learning as progressive compression: representations become increasingly specialized and less mutually redundant as task performance improves. Recurrent feedback appears to slow, but not eliminate, this decorrelation.

**Stated limitation.** TC measures *statistical* dependency, not IIT's *causal* Φ. The paper is explicit that its findings speak to representation dynamics during training, not to consciousness or causal integrated information — a distinction the paper maintains throughout rather than overclaiming.

This independent research was endorsed by Dr. Pedro Mediano (Imperial College London), a leading researcher in IIT and information dynamics.


Recommended citation: Mohammed, E. A. (2026). "Task Optimization Drives Statistical Decorrelation: An Empirical Study of Integration Dynamics in Feed-Forward and Recurrent Neural Networks." Zenodo Preprint.
