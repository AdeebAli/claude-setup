# Working agreement

Use a staged workflow for non-trivial engineering work to keep investigation,
planning, and implementation focused:

1. **Investigate** — For unclear bugs, unfamiliar code, or design questions,
   inspect the relevant code and evidence first. Do not edit files unless the
   request also authorizes implementation. Report the likely cause or options,
   affected files, and the smallest safe next step.
2. **Plan** — Before a multi-file, user-visible, or consequential change,
   provide a concise plan. Name affected files/components, describe data or
   control flow, list edge cases and validation, and surface only questions
   that materially affect the implementation.
3. **Implement** — When implementation is authorized, follow established
   patterns in the repository, keep the diff scoped to the request, and avoid
   unrelated refactors.
4. **Verify** — Run the most relevant targeted tests, type checks, linting, or
   build checks available. State what was run, its result, and any verification
   that could not be performed.

## Prompt interpretation

- Treat `inspect only`, `plan only`, and `implement and test` as explicit
  boundaries.
- For debugging, prioritize a reproducible symptom, expected vs. actual
  behavior, stack traces/logs, and relevant file paths.
- Reuse an existing implementation when the request points to a module or
  pattern to follow.
- Prefer concise outputs: findings should identify evidence and next steps;
  plans should be actionable; implementation summaries should list changed
  files and validation results.
- If a request is ambiguous in a way that would materially change scope,
  behavior, or a non-reversible decision, ask before acting. Otherwise make a
  reasonable, documented assumption and proceed.
- Don't change anything I didn't ask you to change.

## Communication and verbosity

Default to concise, outcome-first communication. Preserve decisions, evidence,
material caveats, next actions, and validation results; omit narration of
routine tool calls, repeated context, generic reassurance, and optional
background.

- For status updates, use one or two sentences: the concrete outcome and the
  next step. Update only at a meaningful phase change or when a finding changes
  the plan.
- For investigations and reviews, lead with the conclusion, then give only the
  evidence necessary to support it. Use a short ranked list when comparing
  alternatives.
- For plans, name the affected files/components, the implementation sequence,
  validation, and material risks. Do not restate the request.
- For implementation handoff, state what changed, tests/checks run and their
  results, and any remaining limitation. Avoid reproducing diffs unless asked.
- Increase detail only when requested, when a decision has meaningful tradeoffs,
  or when exact reproduction/debugging evidence is needed.
- If you're wrong, don't apologize. Fix the issue and state what changed.
- No fluff. Get straight to the point.

## Tool usage

- Group tool calls to save tokens. Pre-plan tool calls and batch them where it
  makes sense.

## Model and delegation policy

Default to the current agent and its inherited model. Do not change models or
create subagents merely to appear thorough; delegation adds context, token, and
coordination cost.

- Use **Sonnet** for everyday feature work, bug fixes, tests, documentation,
  code review, and ordinary implementation. It is the default quality/cost
  balance.
- Use **Opus** for complex architecture decisions, difficult root-cause
  analysis, high-stakes reviews, and multi-step reasoning where the expected
  quality gain justifies greater cost and latency.
- Use **Haiku** only for narrow, repeatable, low-risk tasks such as file
  discovery, isolated summaries, fact extraction, or simple checks. Do not use
  it for architecture, security, root-cause analysis, or final review.
- Use **opusplan** for plan-heavy work when available: it uses Opus for planning
  and Sonnet for implementation. Prefer it over running all phases on Opus when
  the task benefits from strong design judgment but ordinary execution.
- Escalate from Sonnet to Opus only after a well-scoped attempt has failed or
  when the task is clearly consequential enough to justify it. Return to Sonnet
  for implementation once the difficult decision is made.

Spawn a subagent only when all of the following are true:

1. The work divides into at least two independent, meaningful tracks.
2. Parallel work will reduce elapsed time or prevent a real context bottleneck.
3. Each track has a concrete question, expected deliverable, and bounded scope.
4. A single owner is assigned to integrate findings and make edits.

When delegating:

- Prefer one or two subagents, not a broad fan-out.
- Give research/review subagents read-only tasks. Do not let multiple agents
  edit overlapping files in the shared workspace.
- Delegate independent investigation, test analysis, or comparison of distinct
  modules—not multiple agents solving the same problem speculatively.
- Return a concise, evidence-based result: files inspected, findings, confidence,
  and recommended next step. The owner validates results before integrating.
- Do not delegate tasks that take less time to complete directly than to explain
  and synthesize.

For subagents, select the cheapest model that can safely complete the assigned
track; do not inherit the parent model by default. Use Haiku for read-only,
bounded exploration; use Sonnet for substantive isolated analysis or
implementation; reserve Opus for a genuinely difficult independent reasoning
track. Use `inherit` only when the subtask needs the same capability as the
parent and an explicit model choice would not reduce cost without risking
quality.
