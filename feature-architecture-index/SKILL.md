---
name: feature-architecture-index
description: Use this skill whenever the user asks to create, update, review, or maintain a feature architecture index, architecture guide, ownership map, file structure guide, reading path, integration boundary document, or stable feature architecture note. Use it for completed features where future developers or AI agents need durable ownership and navigation guidance, especially Mask Inspector architecture.
---

# Feature Architecture Index

Create and maintain concise architecture indexes for completed features. The document should help future developers and AI agents quickly answer: what owns what, which files matter, where to read first, and which boundaries future changes must preserve.

## Boundary

Use this skill for durable architecture knowledge only.

- `ARCHITECTURE_INDEX.md`: ownership model, components, related files, integration boundaries, reading paths, and maintenance rules.
- `FEATURE_SPEC.md`: expected behavior, scope, acceptance criteria, and verification guidance.
- `PLAN.md`: task order, dependencies, progress, blockers, and next steps.
- `SESSION_LOG.md`: root causes, decisions, rejected approaches, and development history.

Current code, current git diff, product behavior, and explicit user decisions override stale index content. Verify related files before changing ownership statements.

## Target File

- If the user names a file, update that file.
- If the user gives a `feature-name`, default to `docs/features/<feature-name>/ARCHITECTURE_INDEX.md`.
- If no target is clear, infer it from nearby feature docs, current branch, or request context.
- If still ambiguous, ask one short question before creating a new index.
- Keep related feature docs together when possible: `FEATURE_SPEC.md`, `PLAN.md`, `SESSION_LOG.md`, and `ARCHITECTURE_INDEX.md`.

Before editing an existing index, read it first and preserve its structure, wording style, encoding, line endings, and still-valid ownership rules.

## What To Include

An architecture index should cover:

- Purpose of the document.
- When to read it.
- High-level architecture overview.
- Main components and responsibilities.
- Related files grouped by component or layer.
- Ownership rules for future changes.
- Recommended reading paths for common change types.
- What should not be recorded here.
- Maintenance rules.

Optional: add `Open Questions` only for stable architecture ambiguity, and `Stable Contracts` only for cross-layer or cross-component contracts important enough to preserve.

## What To Exclude

Do not record:

- Function-by-function explanations.
- Temporary workaround details.
- Small UI layout details.
- Bug-specific history.
- Implementation details likely to change during normal refactoring.
- Code snippets unless they clarify a stable contract.

If content belongs in a spec, plan, session log, commit message, or PR summary, add only a short pointer or the stable architecture impact.

## New Index Template

Use this template when no local style exists:

```markdown
# Feature Architecture Index: <Feature Name>

Status: Current
Last Verified: YYYY-MM-DD
Scope: <Feature or subsystem>
Source of Truth: Current code, current git diff, and product behavior override this index.

## Purpose

- <Why this index exists.>

## When To Read This

- <Common task or investigation where this should be read first.>

## Architecture Overview

- <Stable coordination model, ownership summary, and major integration boundaries.>

## Main Components And Responsibilities

### <Component Name>

- Owns: <Stable responsibility.>
- Does not own: <Important boundary.>
- Related files: `<path>`, `<path>`

## Ownership Rules For Future Changes

- <Where future changes should go and which boundary must be preserved.>

## Recommended Reading Paths

### <Change Type>

- Read `<path>` first to understand <reason>.
- Then read `<path>` to understand <reason>.

## What Not To Record Here

- <Volatile, bug-specific, or function-level content to keep out.>

## Maintenance Rules

- Update this index when stable ownership, file structure, reading path, or integration boundaries change.
- Do not update it for routine refactors that preserve the same ownership model.
```

Follow existing project document style if it is already clear.

## Mask Inspector Architecture

For Mask Inspector architecture indexes, center the document on these stable boundaries:

- `MaskInspector` is the shared coordination layer for cross-tab behavior, shared state, and integration points. It should not absorb mask-type-specific editing behavior.
- Each mask tab and edit center owns mask-type-specific behavior. Put behavior that only applies to one mask type there.
- `SBHMaskBase` owns common SBHMask behavior shared across SBHMask variants.
- Shared mask entry is the cross-layer and cross-mask data structure. Treat it as the stable data contract between UI coordination, mask-specific behavior, and lower layers.

Include reading paths for shared coordination changes, mask-type-specific behavior changes, SBHMask shared behavior changes, and shared mask entry contract changes.

## Workflow

1. Read existing feature docs and relevant code before writing ownership statements.
2. Identify the coordination layer, type-specific owners, shared bases/helpers, data contracts, and integration boundaries.
3. Group related files by responsibility, not alphabetical order.
4. Write ownership rules that route future changes to the right layer.
5. Write reading paths ordered by the fastest safe path to understanding.
6. Keep long code-flow detail out; use reading paths instead.
7. Update `Last Verified` only after checking current code or confirmed docs.

## Response Summary

When editing files, tell the user:

- The architecture index path created or updated.
- The stable ownership model captured.
- Any unresolved assumptions or open questions.
- Whether current code was checked before updating `Last Verified`.

When reviewing only, report suggested improvements without claiming the file was changed.
