---
name: git-workflow
description: Keep git history clean and collaboration smooth — small commits, good messages, sane branching, and safe rebases. Use when committing, branching, reviewing, or untangling git.
---

# Git Workflow

Git history is documentation and a debugging tool (`git bisect`, `git blame`). Treat commits as a narrative of *why*, not a dumping ground.

## When to Activate
- Committing, branching, or opening a PR
- Cleaning up messy history before merge
- Untangling a git mess (conflicts, wrong branch, bad rebase)

## Commits
- **Small, atomic, one logical change** each. A commit should be revertable on its own and pass tests.
- **Separate refactors from behavior changes** (own commits) — keeps review and `bisect` sane.
- **Message = why, not what.** Imperative subject ≤ ~50 chars ("Fix race in token refresh"), blank line, body explaining the reason and trade-offs. The diff shows *what*; the message must give *why*.

## Branching
- **Short-lived feature branches** off main; merge often to avoid drift.
- **Trunk-based / small PRs** beat long-lived branches that rot into merge hell.
- Name branches meaningfully (`fix/token-refresh-race`).

## Merging & rebasing
- **Rebase your *local, unpushed* branch** to keep history linear and clean; **never rebase shared/pushed history** others have (rewrites their base).
- Prefer squash-merge for a tidy main when a PR is many WIP commits; keep meaningful history when commits are already clean.
- Pull with rebase (`pull --rebase`) to avoid noise merge commits.

## Safety
- **`git status`/`git diff` before committing** — don't blind `add -A`; don't commit secrets, `.env`, or build artifacts (use `.gitignore`).
- Recover with `git reflog` — almost nothing is truly lost.
- Use `git bisect` to find the commit that introduced a bug (why atomic commits matter).

## Checklist
- [ ] Commits small, atomic, tests-passing
- [ ] Refactor vs. behavior changes separated
- [ ] Messages explain *why*, imperative subject
- [ ] Short-lived branches; small PRs
- [ ] Only rebase unpushed history; no secrets committed
