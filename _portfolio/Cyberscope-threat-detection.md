---
title: "CyberScope AI — Real-Time Network Threat Detection"
excerpt: "An ML system that classifies network attacks (DoS, Probe, and more) in real time, with an interactive Flask/Gradio interface for security analysis workflows.<br/><img src='/images/cyberscope-portfolio.png'>"
collection: portfolio
---

**Stack:** 
*  Scikit-learn
*  Flask
*  Gradio
*  Python

Unlike the MRI and crop projects, this one doesn't have a paper behind it — it's a standalone engineering build focused on making a threat-classification model usable, not just accurate.

**What I built:**
- A multi-class classifier trained to distinguish network attack types (DoS, Probe, and others) from traffic-level features.
- A real-time prediction pipeline, so classification happens on live/streamed input rather than only in batch evaluation.
- Two interactive front ends — a Flask app and a Gradio interface — so the model is something a security analyst could actually click through, not just a notebook cell.

**Why it's here:** a model that only exists as an offline accuracy score isn't finished, in my view. This project was about closing that last gap between "the classifier works" and "someone who isn't me can use it."

[Code on GitHub](https://github.com/SAYED-ZALABIYA/Cyber-Security-Threat-Detection-Using-Machine-Learning-CyberScope-AI-)
