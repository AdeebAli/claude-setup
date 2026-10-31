---
name: handoff
description: Summarize the current session (decisions, changes, open threads, next steps) into a handoff note before /clear or ending work. Use when the user wants to wrap up, pause, or hand off a session.
argument-hint: "[optional file path to save the note]"
---

# Session handoff

Write a handoff note a fresh session (or another engineer) can pick up cold. Be concise; omit narration.

## Contents
1. **Goal** — what this session set out to do (one line).
2. **Status** — done / in progress / blocked.
3. **Decisions** — choices made and why, including rejected alternatives that matter.
4. **Changes** — files changed (`git status` / `git diff --stat`), branch, commits, PRs.
5. **Verification** — what was run and the results; what wasn't verified.
6. **Open threads** — unresolved bugs, questions, TODOs, with `file:line` where relevant.
7. **Next steps** — ordered, concrete actions for the next session.
8. **Resume prompt** — a one-paragraph prompt to paste into a new session.

## Output
- If a path was given ($ARGUMENTS), write the note there.
- Otherwise print it in chat. Offer to save it as `HANDOFF.md` in the repo root.
