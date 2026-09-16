---
name: model-evaluation
description: Evaluate ML/LLM models honestly — right metrics, proper splits, baselines, and error analysis. Use when assessing a model, comparing approaches, or debugging poor real-world performance.
---

# Model Evaluation

A model is only as trustworthy as its evaluation. Most "great" models fail in production because the eval was flawed — leaked data, the wrong metric, or no baseline. Evaluate to predict real-world behavior, not to feel good.

## When to Activate
- Assessing a model or comparing approaches
- A model looks great offline but fails in production
- Choosing a metric or setting up an eval harness

## Get the setup right (this is where evals lie)
- **Held-out test set, used once.** Train / validation / test — tune on validation, touch test only at the end. Reusing test to tune = overfitting to it.
- **No leakage.** Fit all preprocessing on train only; for temporal data, split by time (never random) so you don't test on the future.
- **Representative test set** — matches production distribution. If prod is 1% positives, a balanced test set lies to you.

## Pick metrics that match the goal
- **Classification:** accuracy is misleading on imbalanced data — use precision/recall/F1, and PR-AUC/ROC-AUC. Choose based on the cost of false positives vs false negatives.
- **Regression:** MAE/RMSE (RMSE punishes big errors); R² for variance explained.
- **Ranking/retrieval:** precision@k, recall@k, NDCG, MRR.
- **LLMs:** task-specific evals (exact-match, rubric/LLM-as-judge, faithfulness) on a curated set — not vibes.

## Beyond the single number
- **Baseline first** — a trivial model (majority class / simple heuristic). If you can't beat it, stop.
- **Error analysis** — look at actual mistakes; find patterns and slices where it fails (fairness, edge cases). Aggregate metrics hide segment failures.
- **Confidence/variance** — report across seeds/folds; one run isn't a result.

## Checklist
- [ ] Clean train/val/test; test used once; no leakage
- [ ] Test set representative of production
- [ ] Metric matches the real objective (imbalance considered)
- [ ] Beaten a sensible baseline
- [ ] Error analysis on real mistakes + per-slice performance
