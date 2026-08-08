---
title: "Crop Recommendation System for Egyptian Agriculture"
excerpt: "A tuned Random Forest pipeline that recommends crops from soil and climate data, benchmarked against three baseline models with SHAP-based interpretability.<br/><img src='/images/Screenshot 2025-03-17 000232.png'>"
collection: portfolio
---

**Stack:** 
*  Scikit-learn
*  SHAP
*  SMOTE
*  GridSearchCV
*  Pandas

The engineering build behind the [Crop Selection](/publications/) paper — from a 2,201-record raw dataset to a deployable recommendation pipeline.

**What I built:**
- A preprocessing pipeline: missing-value imputation, MinMaxScaler normalization, label encoding, SMOTE for class imbalance across 22 crop types, and engineered soil–climate interaction features.
- A GridSearchCV + 5-fold cross-validation tuning loop over Random Forest hyperparameters (n_estimators, max_depth, min_samples_split/leaf).
- A benchmarking harness that runs Decision Tree, Linear Regression, and SVM through the identical pipeline — so the final model's numbers mean something relative to real alternatives, not just in isolation.
- A SHAP interpretability layer on top of the final model, surfacing per-feature and per-class contribution plots.

**Result:** Random Forest reached 100% accuracy and F1 = 1.00 (99.38% mean under 5-fold CV), with zero confusion-matrix errors across all 22 classes — while a Decision Tree hitting the same 100% training accuracy collapsed to F1 = 0.32, exposing overfitting the accuracy number alone would have hidden.

[Code on GitHub](https://github.com/SAYED-ZALABIYA/Optimizing-Crop-Selection-Using-Machine-Learning-for-Sustainable-Agriculture-in-Egypt) · [Full paper](/publications/)
