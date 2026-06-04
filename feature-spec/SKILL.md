---
name: feature-spec
description: Use this skill whenever the user asks to create, update, review, or maintain a feature spec, behavior spec, UI behavior requirement, acceptance criteria document, product behavior note, or feature-level requirements file. It keeps stable expected behavior separate from implementation plans, task breakdowns, session logs, commit messages, and architecture guides.
---

# Feature Spec

Create and maintain lightweight feature or behavior specs that describe what the product should do.

Core principle: a feature spec records stable expected product behavior, scope, acceptance criteria, agent-checkable validation contracts, manual verification guidance, and unresolved requirement questions. It is not an implementation diary, task plan, session log, commit message, or architecture guide.

## Use This Skill For

- Creating or updating a feature spec, behavior spec, UI behavior spec, product requirement note, or acceptance criteria document.
- Reviewing an existing spec for unclear behavior, weak acceptance criteria, misplaced implementation details, or missing verification guidance.
- Defining expected behavior, edge cases, out-of-scope behavior, acceptance criteria, or stable rules future tasks must follow.
- Separating feature requirements from implementation plans, task trackers, session logs, architecture notes, commits, and PR summaries.

Do not use this skill when the user only asks for a task list, implementation plan, development session log, commit message, PR summary, or code review.

## Document Boundary

- `FEATURE_SPEC.md`: expected behavior, scope, acceptance, and verification guidance.
- `PLAN.md` or task tracker: task order, dependencies, progress, and current next steps.
- `SESSION_LOG.md`: root causes, important decisions, rejected approaches, and session history.
- Architecture notes or current guides: durable system design knowledge.

If content belongs elsewhere, do not duplicate it in the spec. Record only the requirement or verification impact, or mention that the related document may need an update.

## Source Of Truth

A spec is a behavior guide, not final proof of current implementation.

- Re-check current requirements, explicit user decisions, current code, git diff, and product behavior when relevant.
- If the spec conflicts with current behavior or a user decision, record an `Open Questions` item or ask for a decision.
- Update `Last Verified` only after checking against current requirements, code, or product behavior.

## Before Writing

Find the target spec before editing.

- If the user names a file, update that file.
- If the user provides a `feature-name`, use `docs/features/<feature-name>/FEATURE_SPEC.md` by default.
- If a feature directory has an obvious `FEATURE_SPEC.md`, update it.
- If no target is clear, ask one short question for the path or `feature-name` before creating a spec.
- Do not create a spec only because implementation started; create or update one only when stable expected behavior is worth preserving.

Feature document location rules:

- Use kebab-case for `feature-name`, for example `layer-thumbnail-flow`.
- Default feature directory: `docs/features/<feature-name>/`.
- Default feature spec path: `docs/features/<feature-name>/FEATURE_SPEC.md`.
- Keep related feature documents together: `FEATURE_SPEC.md`, `PLAN.md`, and `SESSION_LOG.md` should share the same feature directory when they describe the same work.

Before changing an existing spec:

- Read it first.
- Preserve its structure, wording style, encoding, and line endings.
- Preserve still-valid requirements.
- Do not remove or weaken behavior unless the user confirms the change.

## Spec Layers

Separate product behavior from verification guidance.

- `Expected Behavior`: stable user-facing or system behavior. Prefer `The user can...`, `The UI should...`, and `The system should...` over implementation-first statements like `Modify X file...`.
- `Acceptance Criteria`: observable conditions that confirm behavior is correct through product behavior, manual testing, automated tests, or code-readable checks.
- `Agent-Checkable Validation`: code-readable contracts an AI agent can inspect or trace, such as event contracts, shared state, routing, ownership, priority, fallback, suppression conditions, and invariants.
- `Manual Verification`: behavior that requires product interaction or visual confirmation, such as cursor behavior, redraw timing, z-order, hover/capture interaction, mouse behavior, and end-to-end UI flow.
- `Open Questions`: requirement or verification ambiguity that blocks confident implementation or review.

For UI-heavy specs, do not claim full visual correctness from code inspection alone. Split verification into user-facing acceptance criteria, agent-checkable contracts, and manual verification.

## New Spec Template

Use this template when the project has no existing spec style:

```markdown
# Feature Spec: <Feature Name>

Status: Draft
Last Verified: YYYY-MM-DD
Owner:
Related task / bug / branch:
Source of Truth: Current requirements, explicit decisions, code, git diff, and product behavior override stale spec content.

## Purpose

- <Why this feature or behavior exists.>

## Expected Behavior

- <Stable user-facing behavior rule.>
- <Important UI state or system behavior rule.>
- <Important edge case or boundary rule.>

## Out Of Scope

- <Behavior this spec does not cover.>

## Acceptance Criteria

- <Observable condition that confirms the feature behavior is correct.>
- <Regression case that must pass.>

## Agent-Checkable Validation

- <Event, state, routing, ownership, fallback, priority, suppression, or invariant that can be checked from code.>

## Manual Verification

- <Visual, cursor, redraw, z-order, hover/capture, mouse, or end-to-end UI behavior that requires product interaction.>

## Open Questions

- <Requirement or verification ambiguity that needs a user/product decision.>
```

Status values:

- `Draft`: proposed, inferred, or not fully confirmed.
- `Current`: confirmed by the user, product requirement, current code, or verified product behavior.
- `Historical`: retained for reference but should not guide new work.

If the project already has a different spec template, metadata format, or heading style, follow the existing style. Do not force every old spec to include every section; add `Agent-Checkable Validation` and `Manual Verification` only when they improve clarity.

## Writing Rules

Write specs for stable behavior and future verification.

- Keep entries concise, structured, and AI-friendly.
- Prefer clear requirement statements over long prose.
- Prefer stable contracts over fragile code-path details.
- Include out-of-scope boundaries and open questions when they affect requirements or verification confidence.
- Keep detailed code changes, debug findings, task breakdowns, progress checklists, rejected approaches, root-cause history, long architecture explanations, and procedural QA plans out of the spec.
- Mention implementation details only when they clarify a requirement or verification boundary.
- Suggest `PLAN.md`, a session log, architecture note, or QA checklist when the content belongs there.

## Updating Workflow

When updating a spec:

1. Read the existing spec.
2. Determine whether the change affects expected behavior, scope, acceptance criteria, validation contracts, manual verification, or open questions.
3. If the change is only task planning, session history, architecture detail, commit summary, or QA procedure, route it to the appropriate document instead of expanding the spec.
4. Update confirmed behavior and acceptance criteria in place.
5. Add or update `Agent-Checkable Validation` only for useful, reasonably stable code-readable contracts.
6. Add or update `Manual Verification` for behavior that cannot be confidently proven by code inspection.
7. If behavior is uncertain or conflicts with current code/product behavior, record an `Open Questions` item or ask for a decision.
8. Do not claim manual UI behavior was verified unless an actual manual or automated UI test was performed.

## Examples

Detailed examples live in `references/examples.md`.

Read that file when creating a UI-heavy spec, resolving ambiguity between requirements and verification guidance, or when the user asks for examples. Do not make every spec highly detailed unless the behavior needs it.

## Output To User

When reviewing only, report suggested improvements without claiming the file was changed.

When editing or creating a spec, briefly report:

- Which spec file was updated or created.
- What requirement areas changed.
- What validation or verification areas changed.
- Any open question or user decision still needed.

Do not paste the full spec unless the user asks.

## Quality Bar

A good feature spec is short, testable, stable enough to guide future tasks, explicit about expected behavior, clear about verification limits, and separate from task execution, development history, detailed test procedures, and architecture knowledge.
