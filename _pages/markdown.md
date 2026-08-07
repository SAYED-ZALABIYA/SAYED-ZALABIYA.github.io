---
permalink: /markdown/
title: ""
author_profile: true
---

<div align="center">
<img width="600" height="289" alt="Basmallah-4-White-940x453" src="https://github.com/user-attachments/assets/d3937692-adaa-4eb2-9998-c55c384c9a81" />
</div>

A short note on how I actually run projects, for anyone deciding whether to collaborate or trust a number in one of my papers.

## Reproducibility comes before results

Every experiment I report is run across multiple random seeds, not one lucky run. In the decorrelation study, both the MLP and RNN results are averaged with variance reported — a single training curve isn't evidence on its own. Code for each published result is public (see the links on each [publication](/publications/) page); if a number in a paper can't be regenerated from the repo, I consider that paper unfinished.

## I report negative results on purpose

The MRI reconstruction ablation showed the generator *alone* performed worse than doing nothing (−19.18 dB vs. −1.42 dB zero-fill). That's not a footnote I buried — it's the headline finding, because it tells the next person building on this work exactly where not to spend their compute. Same instinct applies to the crop recommendation project: reporting the Decision Tree's F1 = 0.32 next to Random Forest's F1 = 1.00, on the same 100%-accuracy pipeline, is what actually proves the Random Forest result means something.

## Benchmarks over single numbers

I don't publish a model's accuracy in isolation. Every applied project in my [portfolio](/portfolio/) is compared against at least one honest baseline — zero-fill for MRI, three other classifiers for crop recommendation — on the identical pipeline. A number without a baseline is a claim; a number next to a baseline is evidence.

## Stack

Python, PyTorch/TensorFlow, and Scikit-learn for modeling; SHAP for interpretability; Weights & Biases for experiment tracking; GCP / Hugging Face for training and deployment. For anything touching a causal or theoretical claim (like the IIT-adjacent work), I try to state the limitation as precisely as the result — Total Correlation is a statistical proxy, not IIT's Φ, and I say so every time, not just in the limitations section.

## Working together

If you're looking at a result on this site and want the code, data splits, or seed list to check it yourself, [email me](mailto:elsayednassare525@gmail.com) — I'd rather send you the raw numbers than have you take my word for it.
