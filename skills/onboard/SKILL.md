---
name: onboard
description: Map an unfamiliar repository (architecture, entry points, build/test commands, gotchas) and write the findings to its CLAUDE.md. Use when starting work in a new or unfamiliar codebase.
argument-hint: "[repo path, defaults to current directory]"
---

# Onboard: $ARGUMENTS

Read-only until step 4.

## 1. Survey
- README, CONTRIBUTING, docs/, existing CLAUDE.md / AGENTS.md.
- Manifests and tooling: package.json, pyproject, go.mod, Cargo.toml, build.gradle/pom.xml, Makefile, Dockerfile, docker-compose, CI workflows.
- `git log --oneline -20` and the most-changed paths for recent activity.

## 2. Map
- **Stack:** languages, frameworks, key dependencies, runtime versions.
- **Layout:** top-level directories and what each owns.
- **Entry points:** main binaries, servers, CLI commands, routes, workers.
- **Data:** databases, migrations, ORMs, external services.
- **Flow:** how a typical request or job moves through the system (one short paragraph).

## 3. Verify commands
Find and, where safe, actually run: install, build, test (full + single test), lint, typecheck, run locally.
Record only commands confirmed to work, or mark them unverified.

## 4. Write CLAUDE.md
- If a CLAUDE.md exists, show a proposed diff and ask before changing it.
- Keep it short and actionable: commands, layout, conventions, gotchas. No content derivable from a quick `ls`.

## Report
Summary of the stack, the verified commands, open questions, and the gotchas found.
