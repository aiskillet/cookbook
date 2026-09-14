---
name: threat-modeling
description: Systematically find security risks before shipping — map the system, enumerate threats (STRIDE), and prioritize mitigations. Use when designing a feature/system or reviewing its security posture.
---

# Threat Modeling

Threat modeling is asking, before you ship: *what could go wrong, and what will we do about it?* Do it at design time — it's far cheaper than finding out in production.

## When to Activate
- Designing a new feature/system/service
- Reviewing the security posture of an existing one
- Handling sensitive data, auth, payments, or external input

## The four questions (Shostack's frame)
1. **What are we building?** Draw the system: components, data stores, and **data flows across trust boundaries** (where data crosses from less- to more-trusted, e.g., internet→API, service→DB). Trust boundaries are where threats live.
2. **What can go wrong?** Enumerate threats per element with **STRIDE**:
   - **S**poofing — impersonating a user/service → *authentication*
   - **T**ampering — modifying data/code → *integrity*
   - **R**epudiation — denying an action → *logging/audit*
   - **I**nformation disclosure — leaking data → *confidentiality/encryption*
   - **D**enial of service — exhausting resources → *rate limits/quotas*
   - **E**levation of privilege — gaining unauthorized access → *authorization*
3. **What are we going to do about it?** For each real threat: **mitigate, eliminate, transfer, or accept** — and record which.
4. **Did we do a good job?** Review; revisit when the design changes.

## Principles
- **Focus on trust boundaries and data flows** — that's where attacks cross.
- **Prioritize by risk** = likelihood × impact. You can't fix everything; fix what matters.
- **Assume breach** for defense in depth — don't rely on a single control.
- **Keep it lightweight and living** — a whiteboard + a threat list beats a 40-page doc nobody updates.

## Output
A short doc: the data-flow diagram, a table of (element → STRIDE threat → mitigation → status), and the risks you've explicitly accepted.

## Checklist
- [ ] System + data flows + trust boundaries diagrammed
- [ ] Threats enumerated with STRIDE per boundary
- [ ] Each threat: mitigate / eliminate / transfer / accept (recorded)
- [ ] Prioritized by likelihood × impact
- [ ] Revisited when the design changes
