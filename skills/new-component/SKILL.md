---
name: new-component
description: Scaffold a new design system component that follows the repo's existing conventions - component, types, styles with tokens, Storybook stories for every variant and state, tests, and docs.
argument-hint: <ComponentName> [Figma URL] [short description]
disable-model-invocation: true
---

# New component: $ARGUMENTS

Match the existing design system exactly. Conventions come from the repo, not from defaults.

## 1. Learn the conventions
Read 2-3 existing components that are closest in kind (e.g. for a Select, read other form controls), plus the project CLAUDE.md and any contributing docs. Note:
- **Folder and file layout:** names, index/barrel exports, where types, styles, stories, tests, and docs live.
- **Component patterns:** function vs. forwardRef, polymorphic `as`/`asChild`, compound components, controlled/uncontrolled state, prop naming (`variant`, `size`, `intent`, `isDisabled` vs. `disabled`).
- **Styling:** CSS Modules, Tailwind, CSS-in-JS, or vanilla-extract; how tokens are consumed; how variants are expressed (cva, recipes, data attributes).
- **Stories:** CSF version, argTypes, how variants/states are shown, any interaction tests (`play`).
- **Tests:** runner (Vitest/Jest), Testing Library conventions, a11y tests (jest-axe, Storybook a11y addon).
- **Docs:** MDX, autodocs, or a separate docs site.
- **Exports:** where the package entry point registers new components; changesets or other release requirements.

If there are no existing components, stop and ask which conventions to use.

## 2. Design input
- If a Figma URL was given, load the `figma-design-to-code` skill (if available) and pull the node's design context, variants, and variables. Treat it as a reference to map onto existing tokens and patterns, not code to copy.
- Map every design value to an existing token. List any value with no matching token — never hard-code it; ask whether to add a token.

## 3. Plan (confirm before writing)
Present briefly:
- Files to create and existing files to modify (exports, changesets).
- Props API: names, types, defaults; variants and sizes; states.
- The WAI-ARIA APG pattern it follows, if it's interactive, and its keyboard behavior.
- Open questions (missing tokens, ambiguous states).

Wait for approval.

## 4. Implement
- Component and types following the patterns from step 1. Forward refs and spread remaining props onto the root element if existing components do.
- Styles using tokens only. Support every theme the system supports.
- Accessibility: native elements first; roles, states, keyboard interaction, and focus management per the APG pattern; visible focus; `prefers-reduced-motion` respected.
- Stories: one per variant and state, plus a playground with controls. Include an interaction test if the repo uses them.
- Tests: rendering, props/variants, keyboard interaction, controlled/uncontrolled behavior, and an axe check if the repo has one.
- Docs: usage, props table, do/don't, accessibility notes.
- Register the export, and add a changeset if the repo uses them.

## 5. Verify
- Run typecheck, lint, the component's tests, and a Storybook build or test run if available.
- If the agents are available, offer to run `a11y-auditor` on the new stories and `design-reviewer` against the Figma node.

## Report
Files created/changed, props API summary, checks run and results, open questions (missing tokens, design gaps).
