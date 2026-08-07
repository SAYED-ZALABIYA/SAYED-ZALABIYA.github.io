---
title: "Optimizing Crop Selection Using Machine Learning for Sustainable Agriculture in Egypt"
collection: Publications
permalink: /publication/2025-06-01-crop-selection-egypt
excerpt: 'A Random Forest-based crop recommendation system for Egyptian agriculture, benchmarked against Decision Tree, SVM, and Linear Regression, reaching a perfect F1-score with SHAP-based interpretability.'
date: 2025-06-01
venue: 'EKB Journal (co-authored)'
paperurl: 'https://journals.ekb.eg/article_431307.html'
citation: 'Hussein, M. A., Mohammed, E. A., &amp; Mahfouz, A. (2025). &quot;Optimizing Crop Selection Using Machine Learning for Sustainable Agriculture in Egypt.&quot; <i>EKB Journal</i>.'
---

Co-authored with Mohamed Ahmed Hussein and Ahmed Mahfouz, El Shorouk Academy, Cairo.

Agriculture contributes roughly 12% of Egypt's GDP and employs nearly a quarter of its labor force, yet crop selection still leans heavily on generational knowledge rather than data. This paper builds a data-driven crop recommendation model and, importantly, **benchmarks it honestly against simpler baselines** rather than reporting a single model's numbers in isolation.

**Dataset.** 2,201 records covering 22 crop types (Rice, Coffee, and others), each with 7 environmental features — Nitrogen, Phosphorus, Potassium, Temperature, Humidity, Soil pH, Rainfall — sourced from the FAO, Kaggle agricultural datasets, and the Egyptian Agricultural Research Center (ARC).

**Pipeline.** Missing-value imputation → MinMaxScaler normalization → label encoding → SMOTE (to fix class imbalance across the 22 crop types) → feature engineering (soil–climate interaction terms) → stratified 80/20 split → GridSearchCV + 5-fold cross-validation for hyperparameter tuning.

**Model comparison — the paper's core evidence:**

| Model | Accuracy | Precision | Recall | F1-score |
|---|---|---|---|---|
| Decision Tree | 100%* | 0.35 | 0.30 | 0.32 |
| Linear Regression | 96% | 0.40 | 0.35 | 0.37 |
| Support Vector Machine | 97% | 0.30 | 0.25 | 0.27 |
| **Random Forest (proposed)** | **100%** | **1.00** | **1.00** | **1.00** |

*The Decision Tree also reached 100% *training* accuracy, but its F1-score of 0.32 exposes severe overfitting — it memorized training patterns rather than learning generalizable structure. This comparison is used deliberately in the paper as the counter-example that justifies choosing Random Forest.

The Random Forest model — 50 estimators, no max depth, tuned via GridSearchCV — held its 100% accuracy and F1 = 1.00 under 5-fold cross-validation (99.38% mean accuracy) and produced a confusion matrix with **zero off-diagonal errors** across all 22 crop classes.

**Interpretability.** SHAP analysis showed Rainfall (23.5% importance) and Humidity (21.2%) as the dominant predictors, followed by the soil nutrients (K, P, N). Unlike the Decision Tree — which leaned heavily on just 2–3 features, a sign of overfitting — the Random Forest's SHAP values were spread more evenly across all 7 features, consistent with its ensemble nature and its better generalization.

**Practical implication.** The model is proposed as a decision-support tool for farmers and agricultural engineers, with future work aimed at real-time IoT sensor integration and multi-seasonal, multi-regional data.

[Download paper here](http://SAYED-ZALABIYA.github.io/files/crop-selection-egypt.pdf)

Recommended citation: Hussein, M. A., Mohammed, E. A., & Mahfouz, A. (2025). "Optimizing Crop Selection Using Machine Learning for Sustainable Agriculture in Egypt." EKB Journal.
