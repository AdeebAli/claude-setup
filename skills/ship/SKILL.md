---
name: ship
description: Pre-ship gate - lint, typecheck, test, then conventional commit and PR from the repo's template.
argument-hint: "[optional PR title or context]"
disable-model-invocation: true
---

# Ship current changes

Context from user: $ARGUMENTS

## 1. Inspect
- `git status`, `git diff` (staged + unstaged), current branch, default branch.
- If on the default branch, create a feature branch first.
- Flag anything that shouldn't ship: debug logs, commented-out code, secrets, stray files. Ask before excluding files.

## 2. Gate
Detect the project's commands (package.json scripts, Makefile, pyproject, Cargo.toml, go.mod, gradle/maven, CI config) and run:
1. Formatter/linter
2. Typecheck / compile
3. Tests

If anything fails: stop, report the failure output, and propose a fix. Do not commit failing code.

## 3. Commit
- Conventional commit: `type(scope): summary` (feat, fix, refactor, docs, test, chore, perf, build, ci).
- Body explains *why*, not what. Split into multiple commits only if the changes are clearly unrelated.
- Show the message and ask before committing.

## 4. PR
- Look for `.github/pull_request_template.md` or `.github/PULL_REQUEST_TEMPLATE/`; fill it if present.
- Otherwise: Summary, Changes, Testing (what was run + results), Risks.
- Link related issues.
- Ask before pushing and running `gh pr create`.

## Report
Branch, commit hash(es), gate results, PR URL.
