---
name: pr-check
description: Check current branch/PR against the Way of Working and Definition of Done standards before requesting review
allowed-tools: Bash(git *), Bash(gh *), Bash(wc *), Bash(golangci-lint *), Bash(gofmt *), Grep, Glob, Read
disable-model-invocation: true
---

# PR Readiness Check

Validate the current branch against the team's Way of Working and Definition of Done.

Run all the following checks and produce a report:

## 1. Branch Check
- Verify current branch is NOT `main` or `develop`
- Verify branch name follows `<type>/<short-description>` convention (types: feat, fix, chore, docs, refactor, test, ci)
- Check that the branch is based off `main` (not off another feature branch): run `git merge-base --is-ancestor origin/main $(git rev-parse HEAD)` — if this fails, the branch is not based on main

## 2. Commit Check
- List all commits on this branch not on main: `git log origin/main..HEAD --oneline`
- Flag any commits with banned messages: `wip`, `fixes`, `update`, `stuff`, `misc`
- Flag any commits NOT following conventional commit format (`type:` or `type!:` prefix)
- Check if commits are signed: `git log origin/main..HEAD --format='%H %G?'` — flag any showing 'N' (not signed)

## 3. PR Size Check
- Count total changed lines: `git diff origin/main...HEAD --stat`
- If over 400 lines changed, flag as too large with a warning
- If over 200 lines, note as approaching the soft limit

## 4. Test Check
- Look for test files in the diff: `git diff origin/main...HEAD --name-only | grep -i _test`
- If new `.go` files are added but no corresponding `_test.go` files, flag it
- Check if any existing test files were deleted

## 5. Changelog Check
- Check if CHANGELOG.md (or similar) was modified: `git diff origin/main...HEAD --name-only | grep -i changelog`
- If no changelog entry found, flag it

## 6. Lint Check
- Run `gofmt -l .` and flag any unformatted files
- If `golangci-lint` is available, run `golangci-lint run --new-from-rev=origin/main` and report issues

## 7. TODO/FIXME Check
- Search the diff for any new TODO or FIXME: `git diff origin/main...HEAD | grep '^\+.*\(TODO\|FIXME\)'`
- If found, check if they reference a ticket/issue number. Flag any that don't.

## 8. Breaking Change Check
- Search commit messages for `!:` (breaking change indicator)
- If breaking changes found, verify the changelog mentions them

## Report Format

Output a clear pass/fail checklist:

```
## PR Readiness Report

Branch: <branch-name>
Commits: <count> commits ahead of main

| Check              | Status | Details |
|--------------------|--------|---------|
| Branch naming      | PASS/FAIL | ... |
| Based off main     | PASS/FAIL | ... |
| Commit messages    | PASS/FAIL | ... |
| Commits signed     | PASS/FAIL | ... |
| PR size (<400 LOC) | PASS/FAIL | ... lines changed |
| Tests included     | PASS/FAIL | ... |
| Changelog updated  | PASS/FAIL | ... |
| Lint clean         | PASS/FAIL | ... |
| No loose TODOs     | PASS/FAIL | ... |
| Breaking changes   | PASS/N/A | ... |

Overall: READY / NOT READY for review
```

If NOT READY, list the specific items that need fixing.
