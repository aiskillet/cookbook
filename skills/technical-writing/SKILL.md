---
name: technical-writing
description: Write clear technical docs — lead with the reader's goal, be concrete, and structure for scanning. Use when writing docs, guides, RFCs, or explaining a system in prose.
---

# Technical Writing

Good technical writing gets the reader to their goal with the least effort. Optimize for the reader who's scanning and stressed, not for completeness.

## When to Activate
- Writing docs, guides, tutorials, RFCs, or design notes
- Explaining a system, API, or decision in prose
- Editing writing that's dense or rambling

## Principles
- **Lead with the reader's goal**, not your architecture. Answer "what can I do / why do I care" in the first two sentences.
- **Bottom line up front (BLUF).** Conclusion first, then support. Don't make readers scroll for the answer.
- **Concrete over abstract.** Show a real example/command, not just a description.
- **One idea per paragraph.** Short sentences. Cut hedging ("basically", "in order to" → "to").
- **Structure for scanning:** descriptive headings, short paragraphs, lists for steps, tables for comparisons. People skim first.
- **Active voice, present tense, "you".** "Run `x`" beats "the command should be run."
- **Define jargon on first use** or link it. Don't assume your context.

## Structure that works
1. What this is + who it's for (1–2 lines).
2. The fastest path to success (quickstart / the happy path).
3. Details, options, edge cases.
4. Troubleshooting / gotchas.

## Editing pass (ruthless)
- Delete every sentence that doesn't help the reader act.
- Replace abstract claims with a concrete example.
- Read it as someone who's never seen the system — where do you get lost?

## Checklist
- [ ] Reader's goal + payoff in the first lines
- [ ] Conclusion before justification
- [ ] Concrete examples, not just description
- [ ] Scannable: headings, lists, short paragraphs
- [ ] Active voice; jargon defined; filler cut
