---
name: upgrade-deps
description: Upgrade project dependencies safely - inventory outdated packages, read changelogs for breaking changes, upgrade in batches, fix breakages, and summarize risk.
argument-hint: "[package names | 'all' | 'patch' | 'minor' | 'major']"
disable-model-invocation: true
---

# Upgrade dependencies: $ARGUMENTS

## 1. Inventory
- Detect the package manager from lockfiles (npm/pnpm/yarn/bun, pip/uv/poetry, cargo, go, gradle/maven, bundler).
- List outdated packages: current → wanted → latest. Classify each as patch, minor, or major.
- Flag known security advisories (`npm audit`, `pip-audit`, `cargo audit`, etc., if available).

## 2. Plan
- Present the upgrade list grouped by risk. Majors list breaking changes from the changelog/release notes.
- Delegate changelog and migration-guide reading to the `docs-researcher` agent (one call per major, or batched for related packages) so long release notes stay out of this conversation. If it isn't available, read them inline.
- Ask which batches to proceed with before upgrading majors.

## 3. Upgrade in batches
Order: patch → minor → each major individually.
After each batch: install, build, typecheck, test. If a batch breaks:
- Fix call sites per the migration guide, or
- Revert that package and note why.

Keep the lockfile updated and committed alongside the manifest. Don't touch unrelated config.

## 4. Report
Table: package, from → to, breaking changes, code changes made, test result.
List anything skipped or reverted with the reason, and residual risk.
