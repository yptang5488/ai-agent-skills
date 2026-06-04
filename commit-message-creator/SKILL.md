---
name: commit-message-creator
description: Generate a clear engineer-style commit message from the current Git diff, using current session context only when helpful to clarify intent, rationale, naming, or implementation context. Use this skill whenever the user asks for a commit message, commit title, [what]/[why]/[how] message, or wants a Git commit summary from current changes.
---

# Commit Message Creator

Generate a polished Git commit message from the current diff. The diff is the source of truth; session context only clarifies intent, rationale, naming, or final direction.

## Inputs

1. Use staged changes first: `git diff --cached`.
2. If nothing is staged, use tracked modified changes: `git diff`.
3. Use `git status --short --untracked-files=no` only when needed to confirm whether tracked changes exist.
4. Read recent commits only when needed for local commit style, feature terminology, or message format.

If there are no staged or tracked changes, say there are no committable tracked changes instead of inventing a message.

## Grounding Rules

- Describe only behavior implemented in the current diff.
- Follow the diff when it conflicts with session discussion or recent commits.
- Use session context only to improve `[what]`, `[why]`, naming, and implementation intent.
- Prefer the final settled direction over earlier explored or discarded approaches.
- Exclude TODOs, follow-up plans, unimplemented ideas, and future work.
- Do not inspect untracked, ignored, generated, build, cache, vendor, or environment-specific files unless explicitly asked.
- Do not scan the whole repository unless the diff is insufficient to understand the changed code.
- If the diff contains unrelated changes, do not imply one shared purpose; focus on the staged/current dominant completed change and keep wording narrow.

## Optional User-Provided Title

- Preserve or lightly polish it if it matches the diff.
- Refine it if it is vague, too broad, or inconsistent with the diff.
- Prefer the most accurate title based on the actual implementation.

## Output Format

By default, output only the final commit message in a Markdown code block:

```text
[what] <summary>
[why] <reason>
[how]
- <implementation action>
- <implementation action when useful>
```

Do not include explanations, alternatives, notes, or extra sections outside the code block. If the user explicitly asks for a different shape, follow that shape while preserving diff grounding.

## Writing Rules

### `[what]`

- Write an engineering-style title.
- Describe the resulting behavior or technical outcome.
- Avoid vague verbs like "update", "adjust", "improve", or "refactor" unless no clearer wording fits.
- Use a module or feature prefix when useful, such as `[Edit By Chat]`, `[AI Agent]`, `[AI TryOn]`, `[Layer View]`, or `[Design Layer View]`.
- Use Title Case when natural.
- Keep the title compact — do not restate the same concept in two different phrasings within one title.

For bug tickets, use this format when clearly appropriate:

```text
[what]eBug: <bug id> <version> - <module> : <issue>
```

### `[why]`

- Write one sentence.
- Explain the product, UX, or technical reason.
- Do not repeat `[what]` in different words.
- Do not describe implementation details here.

### `[how]`

- Choose enough bullets to explain the change at commit-message level.
- Focus on concrete implementation changes.
- Mention important classes, functions, state, data flow, ownership, or condition handling when they clarify the fix.
- Avoid low-level constants, asset names, helper internals, and exhaustive branch details unless central.
- Prefer component/behavior summaries over step-by-step call flow.
- Group related changes by engineering concern, such as UI placement, command ownership, selection behavior, visibility, enabled state, or event sync.
- Make `how` bullets mutually distinct; if two bullets describe the same lifecycle, ownership, or cleanup concern, merge them into one higher-level bullet.
- Avoid vague bullets like "Improve logic" or "Update behavior".
- Avoid repeating the intent already stated in `[why]`.
- Describe the resulting behavior or chosen approach, not the before→after transformation. Write "Allow X to do Y" or "Use X for Y", not "Change X from A to B so that Y" or "Switch from A to X".
- When multiple files share an obvious category, name the group instead of enumerating every file path.
- **Do not explain the root cause or internal mechanism in a bullet.** If a bullet ends with a clause like "so that X works", "which caused Y", "triggered by Z", or "when A exceeds B", trim it — that reasoning belongs in `[why]` or is implicit from the diff. A how-bullet should read like an action taken, not a bug analysis.
- **Do not describe the before→after code location.** "Move X from A to B" is a structural description of the edit, not a behavioral statement. Reframe as what the code now does: "Initialize X only once" instead of "Move X initialization from onHeaderChange to __init__".
- **Do not include specific implementation values** (e.g., `max(0, ...)`, `'180'`, `== True`) unless the value itself carries semantic meaning to the reader (e.g., a timeout increasing from 30s to 60s where the numbers are the point).

**Counterexample — too detailed / bug-note style:**
```text
- Move `self.iconWidth = 0` from `onHeaderChange` to `__init__` so explicitly set values persist across subsequent `onHeaderChange` calls triggered by layout passes
- Clamp `iconHMargin` to `max(0, ...)` to prevent negative margins when `iconHeight` exceeds item height
```

**Preferred — behavioral, no mechanism explanation:**
```text
- Initialize dropdown icon fields only once so explicit `iconWidth` values survive `onHeaderChange()` refreshes
- Clamp `iconHMargin` in `DropdownMenuColorMask.onCombolist()` to avoid negative top/bottom margins
```

## Technical Wording

Use inline code formatting for internal identifiers: classes, functions, methods, variables, enums, file names, component names, flags, events, or data structures. Keep user-facing product or feature names in plain text unless they are actual identifiers.

## Style Constraints

- Be implementation-aware.
- Prefer one coherent commit theme.
- Prefer specific wording over broad wording.
- Do not write like a release note, PR summary, design note, or code review.
- If the change is temporary, partial, or condition-based, make that clear.
- Preserve project feature names and terminology exactly.
- Final output must be ready to paste.

## Example

```text
[what] Show Aspect-Based Layer Thumbnails
[why] Keep layer thumbnail framing consistent with the active canvas aspect.
[how]
- Add aspect-based thumbnail layout in `LayerStackView`
- Update thumbnail background rendering by selected aspect
- Fit ROI content into the computed thumbnail rect
- Derive `thumbnailSize` from the current canvas size
```
