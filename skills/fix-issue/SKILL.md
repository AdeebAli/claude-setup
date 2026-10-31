---
name: fix-issue
description: End-to-end fix for a GitHub or Linear issue - fetch, reproduce, root-cause, failing test, fix, verify, draft PR.
argument-hint: <issue number | GitHub URL | Linear ID like ABC-123>
disable-model-invocation: true
---

# Fix issue: $ARGUMENTS

Work in stages. Stop and report if any stage is blocked; don't guess past it.

## 1. Fetch
- Number or github.com URL → `gh issue view <id> --comments`.
- Linear ID (e.g. `ABC-123`) or linear.app URL → Linear MCP tools.
- Extract: expected vs. actual behavior, repro steps, logs/stack traces, acceptance criteria.

## 2. Branch
- If on the default branch, create `fix/<id>-<short-slug>`. Never work on main/master directly.

## 3. Reproduce
- Reproduce the symptom (run the failing command, test, or request). Capture the exact output.
- If it can't be reproduced, stop and report what was tried.

## 4. Root cause
- Trace from the symptom to the cause. State the cause in one or two sentences with `file:line` evidence.
- Fix the cause, not the symptom.

## 5. Failing test
- Write a test that fails for the reported reason, following the repo's existing test patterns. Run it and confirm it fails.

## 6. Fix
- Smallest change that fixes the root cause. No unrelated refactors.

## 7. Verify
- Run the new test, the surrounding test file/suite, and the repo's lint + typecheck.
- If a `test-runner` or `code-reviewer` subagent exists, use it for the full suite / diff review; otherwise do it inline.

## 8. Hand off
Report concisely:
- Root cause (with `file:line`)
- Files changed
- Checks run and results
- Draft PR title + body (link the issue with `Fixes #<id>` or the Linear ID)

Ask before committing, pushing, or opening the PR.
