# code-cadence

Claude Code skills for enforcing engineering standards, fast collaboration, and clean code.

## Skills

### `/wow` — Way of Working & Definition of Done
Quick reference for the team's engineering standards. Prints the full document or a specific section.

```
/wow              # full document
/wow commits      # just the commits section
/wow reviews      # just the code review section
```

### `/pr-check` — PR Readiness Check
Validates your current branch against the Definition of Done before you request review. Checks:

- Branch naming and base
- Commit messages (conventional, signed, no junk)
- PR size (soft limit 200-400 lines)
- Tests included for new code
- Changelog updated
- Lint clean (`gofmt`, `golangci-lint`)
- No loose TODO/FIXME without tickets
- Breaking changes properly flagged

```
/pr-check         # run all checks, get a pass/fail report
```

## Installation

### Per-user (all your projects)
```bash
cp -r skills/* ~/.claude/skills/
```

### Per-project (committed to repo)
```bash
cp -r skills/* <your-repo>/.claude/skills/
```

## Customization

Edit the `SKILL.md` files to match your team's standards:

- Approval count and reviewers
- PR size limits
- Review SLA
- Linting tools (swap `gofmt`/`golangci-lint` for your stack)
- Branch naming conventions

## Tech Stack

Built for Go projects but easily adaptable. The `/wow` skill is language-agnostic. The `/pr-check` skill references `gofmt` and `golangci-lint` — swap these for your linter of choice.
