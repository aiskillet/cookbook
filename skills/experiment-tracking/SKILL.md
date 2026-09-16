---
name: experiment-tracking
description: Make ML experiments reproducible and comparable — log params, data version, code, metrics, and artifacts. Use when running ML experiments or you can't reproduce a past result.
---

# Experiment Tracking

"Which run was that, and how do I reproduce it?" is the question that sinks ML projects. Track enough that any result can be reproduced and any two runs compared.

## When to Activate
- Running ML experiments / hyperparameter sweeps
- You can't reproduce or explain a past result
- Comparing models or handing off to a teammate

## Log everything needed to reproduce a run
- **Params / config:** hyperparameters, model architecture, random seeds.
- **Data version:** which dataset + version/hash (data changes silently — pin it).
- **Code version:** the git commit (and dirty-state warning). Reproducibility needs the exact code.
- **Environment:** key dependency versions.
- **Metrics:** training + validation over time, and final eval numbers.
- **Artifacts:** the trained model, plots, sample predictions, config file.

## Principles
- **Reproducibility is the goal** — a run you can't rerun is a dead end. Seed everything; pin data + code + deps.
- **One experiment = one tracked run**, with a clear name/tag and the question it answers.
- **Compare, don't overwrite** — keep every run so you can compare; don't clobber results.
- **Automate the logging** (MLflow, Weights & Biases, or a structured folder + manifest) — manual notes rot immediately.
- **Version data and models**, not just code — they're inputs and outputs of the experiment.

## Practically
- Use a tracking tool; log at run start (config) and throughout (metrics).
- A run should link: config → data version → code commit → metrics → artifacts.
- Record *why* you ran it and what you concluded (see [[decision-logs]]).

## Checklist
- [ ] Params, seeds, data version, code commit logged
- [ ] Metrics tracked over time + final eval
- [ ] Model + artifacts saved and linked to the run
- [ ] Runs named/tagged; kept for comparison (not overwritten)
- [ ] A past run can actually be reproduced
