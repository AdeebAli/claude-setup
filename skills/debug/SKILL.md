---
name: debug
description: Root-cause a bug, crash, test failure, build failure, or unexpected behavior before any fix. Use when something is broken and the cause isn't already proven, or when a previous fix attempt didn't work.
argument-hint: "[symptom, error message, or failing command]"
disable-model-invocation: true
---

# Debug: $ARGUMENTS

No fix until the root cause is confirmed by evidence. Use what's already known in this conversation; don't re-ask or re-run what's established.

## 1. Pin the symptom
- Exact error, expected vs. actual, when it started, environment.
- Read the full stack trace and every warning. Note file:line at each frame.
- If an essential fact is missing and can't be found, ask once, specifically.

## 2. Reproduce
- Find the smallest command, test, or input that triggers it. Run it and capture the output.
- Intermittent? Run it several times; note the failure rate and what varies.
- Can't reproduce → gather more data. Don't guess.

## 3. Gather evidence
- **Recent changes:** `git log -p` / `git blame` on suspect lines, recent dependency or config changes, env differences.
- **Trace the data backward:** where does the bad value originate? Follow callers up until you reach the source.
- **Multiple components** (CI → build → deploy, API → service → DB): instrument each boundary once (what enters, what exits, which config/env is present) to locate the failing layer before digging into it.
- **Compare with working code:** find a similar path that works and list every difference.

## 4. Hypothesize and test
- One hypothesis at a time: "X causes this because Y."
- Test it with the cheapest experiment: targeted test, scratch script in `mktemp -d`, temporary logging. One variable per experiment.
- Ruled out → record it and form a new hypothesis. Don't stack changes.
- Stuck → say what you don't understand rather than guessing.

## 5. Report — stop here unless a fix was requested
```
Root cause: <one or two sentences> — file:line
Evidence: <reproduction + what confirmed it>
Fix: <smallest correct change>
Also affected: <same flaw elsewhere, or "none found">
Regression test: <test to add>
Ruled out: <hypotheses eliminated>
```
If the request was investigate/inspect only, end with the report. Otherwise ask before editing, unless the user already authorized the fix.

## 6. Fix (when authorized)
1. Write a regression test that fails for the identified reason; confirm it fails.
2. Apply one fix at the root cause. No bundled refactors or "while I'm here" changes.
3. Run the regression test, the surrounding suite, lint, and typecheck. Report results.
4. Clean up temporary logging and scratch files.

## Stop rules
- **Fix didn't work:** return to step 3 with the new information. Don't layer another fix on top.
- **Three failed fixes, or each fix surfaces a new problem elsewhere:** stop. Report what you've learned and raise whether the design itself is the problem before trying again.
- **Truly environmental or timing-dependent:** document what was checked, then add proper handling (condition-based waits instead of sleeps, retries, clear errors) and logging for next time.
