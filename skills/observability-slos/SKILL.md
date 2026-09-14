---
name: observability-slos
description: Make systems observable and define SLOs that matter — the right metrics/logs/traces, user-centric SLIs, and alerting on symptoms. Use when instrumenting a service or setting SLOs/alerts.
---

# Observability & SLOs

You can't operate what you can't see. Instrument for the questions you'll ask at 3am, define reliability from the *user's* perspective, and alert on symptoms, not noise.

## When to Activate
- Instrumenting a service (metrics/logs/traces)
- Defining SLOs/SLIs or error budgets
- Fixing noisy or useless alerts

## The three signals (use all)
- **Metrics** — cheap, aggregate, for dashboards + alerts (latency, error rate, throughput, saturation).
- **Logs** — structured (JSON), with a correlation/request id, for the details of one event. Never log secrets/PII.
- **Traces** — follow one request across services to find *where* the time/failure is.

## The golden signals to watch
**Latency, Traffic, Errors, Saturation** — instrument these for every service. Track latency as **percentiles (p95/p99)**, never averages (averages hide the pain).

## SLOs done right
- **SLI = a user-centric measure:** "% of requests served < 300ms and 2xx." Measure what the user feels, not CPU.
- **SLO = the target:** e.g., 99.9% over 30 days. Pick a number you'll actually defend.
- **Error budget = 100% − SLO.** Spend it: if you're within budget, ship features; if you've blown it, stop and fix reliability. This turns reliability into a decision, not a vibe.
- **Don't chase 100%** — it's infinitely expensive and users can't tell.

## Alerting
- **Alert on symptoms (SLO burn), not causes.** "Error rate breaching budget" > "CPU 80%". CPU high with happy users isn't an incident.
- **Every alert must be actionable + urgent.** If no one acts at 3am, it's a dashboard, not an alert. Kill noisy alerts — they cause fatigue and missed real ones.
- Use **burn-rate alerts** (fast burn = page, slow burn = ticket).

## Checklist
- [ ] Golden signals instrumented; latency as p95/p99
- [ ] Structured logs with correlation id; no secrets/PII
- [ ] Tracing across service boundaries
- [ ] SLIs are user-centric; SLOs have error budgets
- [ ] Alerts fire on symptoms, are actionable, and aren't noisy
