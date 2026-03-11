---
name: wow
description: Display the team's Way of Working and Definition of Done standards
allowed-tools: Read
---

# Way of Working & Definition of Done

Print the following standards document for the team. If $ARGUMENTS is provided, only show the relevant section.

---

## Definition of Done

A PR is done when:

- [ ] CI passes (lint, tests, build) — without modifying CI to force it
- [ ] 2 approvals (tech lead + 1 team member), all comments resolved
- [ ] Signed commits, conventional commit format
- [ ] Tests included for new/changed code
- [ ] Changelog updated (breaking changes flagged)
- [ ] Docs updated if behavior changes
- [ ] No TODO/FIXME without a linked ticket
- [ ] Branch deleted after merge

---

## TL;DR

| Topic | Rule |
|-------|------|
| **Branching** | Always off `main`. No stacking. No `develop`. |
| **Commits** | Small, atomic, signed, conventional. |
| **PRs** | <400 lines, one concern, draft until CI green. |
| **Reviews** | **#1 priority.** Respond within 30 min. Check: functionality, tests, naming, non-determinism. |
| **Breaking changes** | `!` in PR title + description + changelog. |
| **Merging** | Rebase or merge, author's choice. Notify on Slack. |
| **Dependencies** | Pinned, lockfiles committed, updates in separate PRs. |
| **Release branches** | `release/vX.Y.Z` only for hotfixes. Tags on release branches. |

---

## Detailed Guidelines

### Branching

- All work branches off `main`. No exceptions. No `develop`.
- No feature stacking — if you depend on an unmerged PR, wait for it to merge.
- Branch naming: `<type>/<short-description>` (e.g., `feat/add-staking-hook`, `fix/audit-low-004`)
- Release branches (`release/vX.Y.Z`) cut from `main` only for hotfixes. Tags go on release branches.
- Auto-delete on merge. Stale branches (2 weeks inactive) cleaned up periodically.

### Commits

- **Small and atomic** — one logical change per commit.
- **Signed** — GPG/SSH signed.
- **Conventional commits**: `feat:`, `fix:`, `chore:`, `docs:`, `refactor:`, `test:`, `ci:`
- **Breaking changes** use `!`: `feat!: remove legacy endpoint`
- Banned in merged code: `wip`, `fixes`, `update`, `stuff`, `misc`.

### Pull Requests

- **Soft limit: 200-400 lines.** One concern per PR. Exceeding 400 lines? Justify or split.
- Open as **Draft** while working. Mark **Ready for Review** only when CI is green.
- Do NOT modify CI config to make it pass.
- Description must include **what** and **why**.
- Breaking changes: `!` in title, explain in body, migration steps if applicable.
- Changelog entry required. Breaking changes flagged explicitly.
- Anyone can merge after approvals — notify on Slack.

### Code Review

**Reviewing PRs is your #1 priority — above writing your own code.**

- First meaningful response within **30 minutes** during working hours.
- **2 approvals** required: 1 from tech lead + 1 from any team member.
- All comments resolved before merge.

**Reviewer checklist:**
- [ ] **Functionality** — does it work as described?
- [ ] **Tests** — present, meaningful, no coverage decrease?
- [ ] **Naming** — clear and consistent?
- [ ] **Security / Non-determinism** — any consensus-breaking or non-deterministic behavior?

Do NOT comment on formatting — that is automated.

### Testing

- New code ships with tests in the same PR.
- No merging that decreases test coverage.
- Critical paths (consensus, state transitions) require integration tests.

### Linting & Formatting

- `gofmt` + `golangci-lint` enforced in CI and pre-commit hooks.
- CI blocks merge on failures.
- Reviewers do not comment on style — if the linter doesn't catch it, it's not worth arguing about.

### Dependency Management

- Pinned to exact versions. Lockfiles (`go.sum`) committed.
- Dependency updates in **separate PRs** — never bundled with feature work.
- CI runs `govulncheck`.

### Observability

- New features must include appropriate logging and metrics.
- Changes to critical paths should update monitoring/alerting.

### Branch Protection on `main`

- Direct push blocked — PR required.
- 2 approvals required.
- CI must pass.
- Signed commits required.
- No force push.
