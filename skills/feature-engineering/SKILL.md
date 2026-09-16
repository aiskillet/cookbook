---
name: feature-engineering
description: Turn raw data into features that make models work — encoding, scaling, leakage prevention, and selection. Use when building an ML model, debugging poor performance, or preparing a dataset.
---

# Feature Engineering

Features usually matter more than the model. Good features make a simple model shine; bad ones sink a fancy one. The cardinal sin to avoid is **leakage**.

## When to Activate
- Building an ML model / preparing a training dataset
- Model underperforms and you suspect the inputs
- Reviewing a pipeline for leakage or bad encodings

## Avoid data leakage (the #1 killer)
Leakage = information at training time that won't exist at prediction time → great offline metrics, useless in production.
- **Fit transforms on train only,** then apply to val/test (scalers, encoders, imputers). Never fit on the full dataset.
- **No target leakage** — don't use features that are proxies for, or computed after, the label (e.g., "was_refunded" to predict churn).
- **Respect time** — for temporal data, never use future information; split by time, not randomly.

## Common transformations
- **Categoricals:** one-hot (low cardinality), target/ordinal encoding (high cardinality, carefully), embeddings (very high).
- **Numeric:** scale/normalize for distance/gradient models; log-transform skewed values; bin when nonlinear.
- **Missing values:** impute deliberately (mean/median/model) **and** add a "was-missing" flag; don't silently drop.
- **Dates:** extract components (day-of-week, month, is-holiday), and deltas.
- **Text/interactions:** TF-IDF/embeddings; cross features that capture interactions.

## Selection & hygiene
- **Drop leaky, constant, and duplicate features.**
- **Prefer fewer, meaningful features** — more isn't better; noise hurts and cost rises.
- **Make it reproducible** — the same transform pipeline at train and serve time (no train/serve skew).
- **Check importance/correlation** to prune and to understand the model.

## Checklist
- [ ] Transforms fit on train only; applied to val/test
- [ ] No target leakage; time respected for temporal data
- [ ] Categoricals/numerics encoded appropriately
- [ ] Missing values imputed + flagged, not dropped silently
- [ ] Same pipeline at train and serve; leaky/constant features removed
