---
name: development-session-log
description: Use this skill whenever the user asks to create, update, append, summarize, maintain, or review a software engineering development session log, change note, session context, implementation diary, debug record, or maintenance handoff note. It preserves session-level engineering knowledge through an updatable Current Summary and an append-only Timeline Log, without replacing code, git history, architecture docs, commit messages, or PR summaries.
---

# Development Session Log

Maintain a concise engineering session log that helps future maintainers understand why decisions were made, what problems were found, and what assumptions changed during development.

Use the log as durable maintenance memory, not as a transcript, architecture document, or commit message. Preserve information that would be hard to reconstruct from the final code diff alone.

## Trust Boundary

Treat the log as a maintenance aid and navigation aid, not a source of truth.

- Current code, current git diff, and current product behavior always take priority over the log.
- Future AI agents and maintainers must verify implementation details against current code before acting on them.
- Record confirmed session knowledge, but do not present the log as authoritative when code or behavior has changed.

## When To Use

Use this skill when the user asks for any of these tasks:

- Create a development record, session log, work log, implementation note, or handoff note.
- Update an existing session context document during coding.
- Append root cause, solution, decision, rejected approach, or superseded assumption notes.
- Summarize the current state of a long debugging or implementation session.
- Keep track of architectural ownership, event flow, pitfalls, or unresolved questions.

Do not use this skill for commit messages, PR summaries, code reviews, or full architecture documentation unless the user also asks to preserve session-level development knowledge.

## Before Writing

Identify the log target before editing:

- If the user names a file, update that file.
- If an obvious existing session log is open or mentioned in the conversation, update that file.
- If the user provides a `feature-name`, use `docs/features/<feature-name>/SESSION_LOG.md` by default.
- If no file is specified, ask one short question for the target path or `feature-name` before creating a new log.

Feature document location rules:

- Use kebab-case for `feature-name`, for example `layer-thumbnail-flow`.
- Default feature directory: `docs/features/<feature-name>/`.
- Default session log path: `docs/features/<feature-name>/SESSION_LOG.md`.
- Keep related feature documents together: `FEATURE_SPEC.md`, `PLAN.md`, and `SESSION_LOG.md` should share the same feature directory when they describe the same work.

Before changing an existing log, read it first and preserve its structure, wording style, encoding, and line endings. Do not rewrite older timeline entries unless the user explicitly asks; append a `Superseded` entry when old understanding changes.

## Document Structure

Each session log has lightweight metadata plus two main parts:

1. Current Summary
2. Timeline Log

Use this template when creating a new log:

```markdown
# Development Session Log

Status: Current
Last Verified: YYYY-MM-DD
Scope:
Related task / bug / branch:
Source of Truth: Current code, current git diff, and current product behavior override this note.

## Current Summary

- Current goal:
- Confirmed implementation direction:
- Important ownership / architecture decisions:
- Known pitfalls:
- Open questions:

## Timeline Log

### YYYY-MM-DD HH:mm - Started

- Context:
- Goal:
```

Template notes:

- `Status` marks trust state: `Current` means the latest summary is still considered valid, `Historical` means retained as history but not current architecture, and `Draft` means the note content itself is not fully confirmed. Open questions alone do not require `Draft` if the current goal and direction are confirmed.
- `Last Verified` is the last date checked against current code or behavior. It does not make the note permanently valid; re-check current code before relying on implementation details.
- `Scope` briefly names the feature, module, bug, or task covered. If the scope grows across unrelated work, suggest splitting the log or moving stable architecture content elsewhere.
- `Related task / bug / branch` links the note to a bug id, task id, branch name, or feature name. Leave it blank if there is no clear source.
- `Source of Truth` is a fixed reminder: the note is a maintenance aid and investigation guide, not a replacement for code inspection.
- `Current Summary` should be short and precise, with only the latest confirmed understanding for this session.
- `Timeline Log` starts with `Started`; append later decisions, root causes, solutions, rejected approaches, and superseded assumptions without rewriting old timeline entries unless the user asks.

If the project already uses a different title, timestamp style, template, or metadata format, follow the existing style instead of forcing this template.

## Current Summary Rules

Current Summary represents the latest confirmed understanding. It may be updated during the session.

Include only:

- Current goal.
- Confirmed implementation direction.
- Important ownership or architecture decisions.
- Known pitfalls.
- Open questions.

Do not include:

- Temporary debug notes.
- Obvious code diff details.
- Repeated project background.
- Unverified guesses.
- Detailed implementation traces.
- Intermediate approaches that have already been replaced.

When updating Current Summary, prefer short bullets. Replace stale summary bullets with the latest confirmed state, but preserve useful unresolved questions. If a point has become stable long-term architecture knowledge, keep only a short pointer here and suggest moving the detail to an Architecture Note or Current Guide.

## Timeline Log Rules

Timeline Log is append-only during a session.

Use it to record:

- Important implementation steps.
- Confirmed root causes.
- Solutions.
- Decisions.
- Rejected approaches.
- Superseded assumptions.
- Non-obvious pitfalls.
- Ownership or event flow changes.

Do not record:

- Every small code edit.
- Every local variable or function rename.
- Temporary print/debug logs.
- Unverified guesses as facts.
- Details obvious from git diff.
- Full transcript-style progress narration.

When an earlier idea becomes invalid, append a `Superseded` entry instead of editing the old entry. When an approach is intentionally not used, append a `Rejected` entry.

## Note Type Boundary

This log records session-level change knowledge only.

- Keep architecture context only when it explains a session decision, pitfall, or ownership boundary.
- Do not expand the log into a full architecture document, onboarding guide, or API reference.
- If the content becomes stable project knowledge, summarize it briefly in Current Summary and recommend moving the durable detail to an Architecture Note or Current Guide.

## Granularity Rule

Record only information that would help future maintenance.

A note is worth keeping if it answers one of these questions:

- Why was this design chosen?
- What problem did we hit?
- What was the root cause?
- What solution fixed it?
- What approach should future maintainers avoid?
- What ownership or event flow changed?
- What pitfall would be hard to rediscover from code alone?

If the note only says what line changed or repeats the diff, omit it.

## Entry Types

Use clear labels when they help future readers scan the timeline:

```markdown
### YYYY-MM-DD HH:mm - Decision

- Chose <direction> because <reason>.
- Impact: <ownership, flow, or maintenance implication>.
```

```markdown
### YYYY-MM-DD HH:mm - Root Cause

- Problem: <observed issue>.
- Root cause: <confirmed cause>.
- Evidence: <optional signal, test result, code path, or observed behavior>.
- Fix direction: <confirmed solution>.
```

```markdown
### YYYY-MM-DD HH:mm - Solution

- Implemented <solution> to address <problem>.
- Impact: <behavior, ownership, or maintenance implication>.
```

```markdown
### YYYY-MM-DD HH:mm - Rejected

- Rejected <approach> because <reason>.
- Prefer <alternative>.
```

```markdown
### YYYY-MM-DD HH:mm - Superseded

- Earlier assumption: <old assumption>.
- Superseded by: <new confirmed understanding>.
- Reason: <evidence or decision>.
```

Use fewer fields for simple entries. Keep entries compact and factual.

## Updating Workflow

When updating a log:

1. Read the existing log and identify the current summary and timeline sections.
2. Determine what information is confirmed versus temporary or speculative.
3. Update Current Summary to reflect the latest confirmed state.
4. Append only meaningful new timeline entries.
5. Preserve old Timeline Log entries exactly unless the user asks for cleanup.
6. If no target file is clear, follow project rules for session logs or ask one short question for the target path.

Writing style:

- Keep entries concise, factual, and AI-friendly: clear labels, short bullets, explicit status, and stable terms.
- Prefer maintenance-useful summaries over detailed implementation traces.
- Update `Current Summary` in place with the latest confirmed understanding.
- Do not overwrite older `Timeline Log` entries; append `Superseded` when prior understanding changes.

## Length Control

Do not let Timeline Log grow without limit.

- Archive older entries when the project has an established archive pattern.
- Compress repeated discoveries into one concise confirmed entry.
- Split logs by feature, bug, or task when one file mixes unrelated work.
- Move stable architecture knowledge to an Architecture Note or Current Guide instead of repeating it here.

Do not delete, heavily rewrite, or compress historical timeline entries without user confirmation.

## Output To User

After editing a log, briefly report:

- Which log file was updated.
- What type of information was added or refreshed.
- Any open question that still needs user confirmation.

Do not paste the full log unless the user asks.

## Quality Bar

A good session log is useful weeks later. It should explain the reasoning behind the current implementation path, preserve hard-won debugging knowledge, and avoid clutter that can be recovered from Git history.
