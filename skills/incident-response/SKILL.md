---
name: incident-response
description: Handle production incidents calmly and effectively — stabilize first, communicate, then fix root cause and run a blameless postmortem. Use during or after an outage.
---

# Incident Response

In an incident, the goal is **restore service first, understand later.** A clear process beats heroics — it reduces downtime and prevents the panic that makes things worse.

## When to Activate
- A production outage/degradation is happening
- Setting up an on-call / incident process
- Writing a postmortem after an incident

## During the incident
1. **Declare it.** Name an **Incident Commander** (coordinates, decides — not necessarily the one typing). Spin up a channel/call.
2. **Stabilize first, root-cause later.** Stop the bleeding: roll back the recent deploy, fail over, scale up, disable the bad feature. Mitigation > diagnosis while users are down.
3. **Communicate on a cadence** — status page + stakeholders, even "still investigating." Silence erodes trust more than bad news.
4. **Assign roles:** commander, ops (hands on keyboard), comms/scribe. Keep a timeline as you go.
5. **Prefer the fast reversible fix** (rollback) over a clever forward fix under pressure.

## After (blameless postmortem)
- **Blameless** — focus on systems and gaps, not people. Blame kills the honesty you need to actually fix things.
- **Timeline:** what happened, when detected, when mitigated, when resolved.
- **Root cause + contributing factors** (5 Whys) — the real cause, not "human error."
- **Action items** with owners + dates that make recurrence less likely (better alerting, guardrails, rollback speed).
- **What went well** too — reinforce it.

## Prep (before it happens)
- Runbooks, clear on-call + escalation, alerts that page on user-facing symptoms, and a practiced rollback.

## Checklist
- [ ] Incident declared; commander named
- [ ] Stabilized first (rollback/failover) before deep diagnosis
- [ ] Communicated on a cadence; timeline kept
- [ ] Blameless postmortem with root cause
- [ ] Action items with owners + dates
