---
name: prompt-engineering
description: Write prompts that are specific, structured, and testable — clear role/task, examples, output contracts, and iteration against evals. Use when writing or debugging an LLM prompt or system message.
---

# Prompt Engineering

A prompt is a spec. Vague specs get vague output. Be specific about the task, the format, and the constraints — then iterate against real examples, not vibes.

## When to Activate
- Writing or debugging an LLM prompt / system message
- Output is inconsistent, wrong format, or ignores instructions
- Designing a prompt that must be reliable in production

## Structure that works
1. **Role + task** — who the model is and exactly what to do.
2. **Context** — the inputs/data it needs (clearly delimited, e.g. in tags or fences).
3. **Constraints** — what to do and *not* do; edge cases.
4. **Output contract** — exact format (JSON schema, sections). Show the shape.
5. **Examples** — 1–3 input→output pairs for anything nuanced (few-shot beats explanation).

## Principles
- **Specific > clever.** "Summarize in 3 bullets, each ≤15 words" beats "summarize nicely."
- **Positive instructions** ("respond only with JSON") beat negatives ("don't add prose").
- **Put the most important instruction first and last** — models weight the ends.
- **Delimit inputs** clearly so the model can't confuse instructions with data (prompt-injection defense).
- **Let it think** for reasoning tasks (ask for steps *before* the answer), then extract the final answer.
- **One prompt = one job.** Chain steps rather than cramming five tasks into one prompt.

## Make it reliable
- **Build a tiny eval set** (10–20 tricky inputs with expected outputs). Change the prompt → run it → compare. This is how you improve without regressing.
- **Pin the output format** and validate it programmatically; reject/repair off-format responses.
- **Version prompts** like code.

## Checklist
- [ ] Role, task, context, constraints, output contract all present
- [ ] Inputs delimited from instructions
- [ ] Examples for anything nuanced
- [ ] Output format is explicit and validated
- [ ] Tested against an eval set, not one happy case
