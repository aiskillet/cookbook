---
name: changelog-writing
description: Write changelogs humans actually read — grouped by change type, user-focused, and versioned. Use when maintaining a CHANGELOG or preparing notes for a release.
---

# Changelog Writing

A changelog is for humans deciding whether/how to upgrade — not a dump of git log. Write what *changed for the user*, grouped and dated.

## When to Activate
- Maintaining a `CHANGELOG.md`
- Preparing notes for a release/version
- Turning commits/PRs into readable change entries

## The format (Keep a Changelog convention)
```
## [1.4.0] - 2026-09-13
### Added
- <new capability, phrased as user benefit>
### Changed
- <behavior change>
### Fixed
- <bug fix>
### Deprecated / Removed / Security
- <as needed>
```
- **Newest on top.** Keep an **`## [Unreleased]`** section you add to as you go, then stamp it with a version + date at release.
- Group by type: **Added / Changed / Deprecated / Removed / Fixed / Security**.

## Principles
- **Write for users, not committers.** "Export now supports CSV" > "refactor exporter module". Skip internal refactors that don't affect users.
- **One entry per meaningful change**, in plain language, present tense.
- **Call out breaking changes loudly** — a dedicated ⚠️ note + migration steps. This is the most important part for anyone upgrading.
- **Link** to PRs/issues for detail; keep the entry itself short.
- **Follow semver:** breaking = major, features = minor, fixes = patch — and the changelog should make the version bump obvious.

## Anti-patterns
- Pasting raw git log / commit hashes.
- "Various bug fixes and improvements" (tells users nothing).
- Only writing it at release from memory — capture as you merge.

## Checklist
- [ ] Grouped by Added/Changed/Fixed/etc., newest first
- [ ] Entries describe user impact in plain language
- [ ] Breaking changes flagged with migration notes
- [ ] Versioned + dated; `Unreleased` kept current
- [ ] Links to PRs for depth
