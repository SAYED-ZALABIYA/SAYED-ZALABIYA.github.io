---
title: 'Hyperparameter Tuning in ML: GridSearchCV vs RandomizedSearchCV (With Math & Strategy)'
date: 2025-07-26
permalink: /posts/2025/07/hyperparameter-tuning/
tags:
  - tutorial
  - ml-engineering
  - hyperparameter-tuning
excerpt: 'Your model isn''t underperforming — you just didn''t tune it yet. A practical, math-first breakdown of GridSearchCV vs RandomizedSearchCV: when to use each, the formulas behind them, and the ranges I actually use in real projects.'
---

*Part of an ongoing series of practical ML engineering notes — the kind of thing I'd want on hand mid-project, not just in a textbook.*
*Original thread posted on [LinkedIn](https://www.linkedin.com/posts/elsayed-a-mohammed-56a669237_deep-dive-into-generative-adversarial-networks-ugcPost-7372158104458838017-2-OQ/?utm_source=social_share_send&utm_medium=member_desktop_web&rcm=ACoAADsCHeIBN5l8lvtkckOBqZXy-qoR1x7AzYo).*
Your model isn't underperforming. You just didn't tune it yet.

You've cleaned your dataset, engineered good features, picked a solid algorithm — and something's still off. Maybe it's underfitting, maybe it's overfitting, maybe it's just "meh." Before you touch the architecture again, the real fix is usually hiding in the hyperparameters.

## 1. What are hyperparameters?

Hyperparameters aren't learned from the data — they're the pre-set configurations that guide how the model learns.

| Algorithm | Hyperparameter examples |
|---|---|
| Random Forest | `n_estimators`, `max_depth` |
| SVM | `C`, `gamma`, `kernel` |
| KNN | `n_neighbors`, `metric` |

## 2. Why tuning matters

Bad values → overfitting or underfitting. Good values → optimized accuracy, precision, recall, everything downstream. The algorithm choice gets most of the attention; the tuning is what actually determines whether that choice pays off.

## 3. How many combinations am I actually testing?

**GridSearchCV** tries every possible combination:

$$\text{Total Fits} = \left(\prod_{i=1}^{k} |H_i|\right) \times cv$$

Example:
```python
param_grid = {
    'n_estimators': [100, 200, 300],
    'max_depth': [5, 10, 15, 20]
}
grid = GridSearchCV(RandomForestClassifier(), param_grid, cv=5)
grid.fit(X_train, y_train)
print(grid.best_params_)
```
3 × 4 = 12 combinations, × cv=5 → **60 total fits**.

**Pros:** exhaustive — finds the best combination within the grid you defined.
**Cons:** very slow if the search space is large.

**RandomizedSearchCV** samples `n_iter` random points from the parameter space instead of trying everything:

$$\text{Total Fits} = n\_iter \times cv$$

```python
from scipy.stats import randint
param_dist = {
    'n_estimators': randint(50, 300),
    'max_depth': randint(3, 30)
}
rand = RandomizedSearchCV(RandomForestClassifier(), param_distributions=param_dist, n_iter=20, cv=5)
rand.fit(X_train, y_train)
print(rand.best_params_)
```
`n_iter=20, cv=5` → **100 total fits**, but over a much larger search space than the grid example above.

**Pros:** much faster; can escape local optima that a coarse grid might miss.
**Cons:** doesn't try every combination — may miss the single best one.

## 4. Quick comparison

| Aspect | GridSearchCV | RandomizedSearchCV |
|---|---|---|
| Type | Exhaustive | Random sampling |
| Time | Slow (combinatorial) | Fast (custom iterations) |
| Best for | Small search spaces | Large search spaces |
| Risk | Overfitting to CV folds | More generalizable |

## 5. How to choose the ranges themselves

Picking the *algorithm* is the easy part — picking sane hyperparameter *ranges* is where people either waste compute or under-search. What I actually use:

| Hyperparameter | Suggested range | Notes |
|---|---|---|
| `n_estimators` | 50 → 500 | Higher = better, slower |
| `max_depth` | 3 → 30 | Low = simple, high = risk of overfit |
| `C` (SVM / LogReg) | 0.01 → 100 (log scale) | Regularization power |
| `learning_rate` | 0.001 → 0.2 | Controls influence range |
| `gamma` (SVM) | 0.0001 → 1 (log scale) | Smaller = local, larger = smoother |
| `n_neighbors` (KNN) | 3 → 20 | Smaller = local, larger = smooth |

For anything spanning multiple orders of magnitude (C, gamma, learning rate), sample on a **log scale**, not linear — otherwise you waste most of your search budget in a range that barely matters:

```python
from scipy.stats import loguniform
param_dist = {
    'C': loguniform(0.01, 100),
    'gamma': loguniform(1e-4, 1e-1)
}
```

## 6. My actual tuning strategy

1. Start wide with `RandomizedSearchCV` to map the rough shape of the search space cheaply.
2. Zoom in with `GridSearchCV` once you have a promising narrow range.
3. Always cross-validate (`cv=5` is my default) — a single train/test split will lie to you about how well tuning generalizes.
4. Use domain knowledge, not just search: small trees for speed, large trees for accuracy, and let the problem tell you which one you actually need before you start searching.

The short version: don't grid-search a huge space, and don't random-search a tiny one. Match the method to the size of the space you're actually exploring.
