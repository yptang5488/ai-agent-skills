---
name: implementation-plan
description: Use this skill whenever the user asks to create, update, review, resume, or maintain PLAN.md, an implementation plan, task breakdown, execution checklist, task order, dependency plan, progress checklist, or current next steps. This skill owns implementation planning only and keeps PLAN.md separate from feature specs, session logs, commit messages, PR summaries, and architecture notes.
---

# Implementation Plan

Create and maintain lightweight `PLAN.md` files that help a developer or AI agent execute work in the right order.

A good plan is current, actionable, easy to resume, and clear about what to do next. It is not the source of truth for feature behavior, historical reasoning, or architecture.

## Use This Skill For

- Creating or updating `PLAN.md`.
- Creating or updating an implementation plan, task breakdown, execution checklist, task order, dependency plan, progress checklist, or next-step guide.
- Resuming multi-step work where the next developer or AI agent needs current execution state.
- Tracking task additions, removals, splits, merges, reordering, blocked work, or deferred work.
- Separating implementation planning from requirements, session history, and architecture notes.

Do not create a standalone plan for a small one-step change unless the user asks. If the task is narrow, obvious, and has no meaningful sequencing or dependencies, handle it directly.

## Document Boundary

`PLAN.md` is an execution guide.

- `PLAN.md`: tasks, order, dependencies, current state, next step, blockers.
- `FEATURE_SPEC.md`: expected behavior, scope, acceptance, validation requirements.
- `SESSION_LOG.md`: root causes, decisions, rejected approaches, useful history.
- Architecture notes: durable system design knowledge.
- Commit messages and PR summaries: final change summaries.

If content belongs elsewhere, do not copy the full details into `PLAN.md`. Record only the planning impact, such as the changed task, dependency, blocker, or related document that may need an update.

## Trust Boundary

Treat the plan as the current execution guide, not an authority.

- Current code, current git diff, product behavior, and linked specs override stale plan content.
- Verify stale unchecked tasks before relying on them.
- If the plan conflicts with current code or a linked spec, record an open question instead of silently choosing.

## Before Writing

Find the target plan before editing.

- If the user names a file, update that file.
- If the user provides a `feature-name`, use `docs/features/<feature-name>/PLAN.md` by default.
- If a feature directory has an obvious `PLAN.md`, update it.
- If no target is clear, infer the `feature-name` from the request, current branch, nearby specs, logs, or existing plans.
- If still ambiguous, ask one short question for the path or `feature-name` before creating a plan.

Feature document location rules:

- Use kebab-case for `feature-name`, for example `layer-thumbnail-flow`.
- Default feature directory: `docs/features/<feature-name>/`.
- Default implementation plan path: `docs/features/<feature-name>/PLAN.md`.
- Keep related feature documents together: `FEATURE_SPEC.md`, `PLAN.md`, and `SESSION_LOG.md` should share the same feature directory when they describe the same work.

Before changing an existing plan:

- Read it first.
- Preserve its structure, wording style, encoding, and line endings.
- Preserve useful task history and plan-change entries unless the user asks for cleanup.
- Verify stale assumptions against current code, git diff, linked specs, or current user instructions.

## New Plan Template

Use this template when the project has no existing plan style:

```markdown
# Implementation Plan: <Feature / Task Name>

Status: Draft
Last Updated: YYYY-MM-DD
Related spec:
Related session log:
Related task / bug / branch:
Source of Truth: Current code, current git diff, current product behavior, and linked specs override this plan.

## Goal

- <What this plan is trying to implement.>

## Current Plan

- <Latest confirmed implementation direction or execution strategy.>

## Task Breakdown

- [ ] <Outcome-oriented task>
- [ ] <Outcome-oriented task>
- [ ] <Outcome-oriented task>

## Dependencies / Order

- <Task, decision, or dependency that must happen before another.>

## Progress Notes

- <Latest resumable state: done, next, blocked, or needs re-check.>

## Open Questions

- <Question affecting sequencing or implementation.>

## Plan Changes

### YYYY-MM-DD - Created

- Created initial implementation plan for <feature / task>.
```

Status values:

- `Draft`: proposed, not confirmed.
- `Active`: current execution guide.
- `Blocked`: waiting on a dependency or decision.
- `Completed`: implementation plan is no longer active.

If the project already has a different plan template, task status style, metadata format, or heading style, follow the existing style.

## Writing Rules

Write for fast execution.

- Use checkbox tasks for work that can be started, completed, blocked, or deferred.
- Name tasks by outcome, not tiny edits. Good: `Route delete requests through shared inspector state`. Poor: `Edit file A`.
- Keep task order and dependencies explicit when order matters.
- Keep `Current Plan` aligned with the latest confirmed strategy.
- Keep `Progress Notes` focused on the latest resumable state: done, next, blocked, and what must be re-checked.
- Keep `Open Questions` actionable and tied to execution.
- Avoid file-by-file diff details, debug diaries, rejected approaches, root-cause history, stable behavior requirements, and vague tasks like `fix issue`.

## Updating Workflow

When updating a plan:

1. Read the existing plan.
2. Confirm the requested change is implementation planning. If it belongs to a spec, session log, architecture note, commit message, or PR summary, record only the planning impact.
3. Verify stale tasks or assumptions against current code, git diff, product behavior, linked specs, or current user instructions.
4. Update task state, order, dependencies, blockers, open questions, and `Current Plan`.
5. Update `Progress Notes` to the latest resumable state, not a chronological transcript.
6. Add a concise `Plan Changes` entry only when execution meaningfully changes.
7. If the plan is becoming long, compress current progress and summarize old plan changes instead of appending diary entries.

Add a `Plan Changes` entry for meaningful execution changes only:

- Task order changed.
- A task was added, removed, split, merged, blocked, completed with follow-up, or deferred.
- Strategy, dependency, scope, or open questions changed in a way that affects execution.

Do not add `Plan Changes` entries for routine checkbox updates, wording cleanup, formatting-only edits, or local implementation details.

## Completing A Plan

Mark a plan `Completed` only when planned implementation work is done or no longer active.

- Check, remove, or explicitly defer every remaining task.
- Leave final `Progress Notes` focused on resumable follow-up, if any.
- Keep the plan `Active` or `Blocked` if implementation is only partially complete.
- Mention if follow-up belongs in a new plan, feature spec, session log, architecture note, or issue tracker.

## Output To User

After creating or editing a plan, briefly report:

- Which plan file was created or updated.
- What changed in task order, dependencies, progress, blockers, or open questions.
- Whether a related spec, session log, architecture note, or issue tracker may need an update.

Do not paste the full plan unless the user asks.

## Quality Bar

`PLAN.md` should be current, actionable, lightweight, easy to resume, clear about order and dependencies, clear about blockers and open questions, and separate from requirements, session history, final summaries, and architecture knowledge.
