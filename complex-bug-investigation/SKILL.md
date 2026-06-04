---
name: complex-bug-investigation
description: Use this skill for bounded first-pass investigation of complex bugs involving multiple features, layers, event flow, state synchronization, lifecycle ownership, async behavior, or unclear ownership. Trigger it when the user asks to investigate, trace, diagnose, find the likely root cause, or understand a complex bug before fixing it. Do not use it for small direct fixes. This skill must stop before implementation and must not edit code.

---

# Complex Bug Investigation

Perform one bounded investigation pass, then stop. This skill is for investigation only.
Do not edit code, implement fixes, or start a review loop.

## Boundary

Start from the most likely entry point and expand only when evidence points to the next file.

- Read only files needed to understand the suspected path.
- Stop if more than 8-12 files seem necessary and explain why scope is expanding.
- Do not map whole feature folders by default.
- Do not inspect unrelated consumers unless they are directly in the failing path.
- Do not continue from investigation into implementation automatically.

## Flow

### 1. Scope

Identify the observed bug, smallest suspected feature boundary, involved layers, assumptions, and unknowns.

Common layers: UI, control, API, state/model, render, async/event, persistence.

### 2. Trace

Trace only the relevant path:

- User action -> entry point -> handler -> state/control/API/render/event.
- Ownership boundaries.
- Lifecycle pairs: create/cleanup, bind/release, show/hide, enable/disable, register/unregister, load/refresh, select/deselect.
- Any place state, lifecycle, or ownership may become inconsistent.

### 3. Diagnose

Produce 1-3 root-cause hypotheses based on the available evidence.

Do not force extra hypotheses when one root cause is already strongly supported.
Do not invent weak hypotheses to satisfy a fixed count.
If more than 3 hypotheses remain plausible, stop and report that the investigation is too broad and recommend the next narrowing step.

For each, include cause, supporting evidence, evidence against, missing evidence, confidence, and next check.

Do not implement or plan a patch in detail. Only recommend the next action.

## Stop Conditions

Stop and report when:

- More than 8-12 files seem necessary.
- More than 3 hypotheses remain plausible.
- No hypothesis has concrete supporting evidence.
- The investigation expands into another feature.
- The bug crosses an unexpected ownership boundary.
- A refactor seems tempting before the root cause is proven.
- No potential fix maps to concrete evidence.

## Output Format

Use this format:

```markdown
### Bug Summary

Describe the observed failure and suspected boundary.

### Flow Trace

Describe the relevant user action -> handler -> state/control/API/render/event path.

### Findings

List concrete observations from the code.

### Hypotheses

Hypothesis 1:
- Cause:
- Supporting evidence:
- Evidence against:
- Missing evidence:
- Confidence:
- Next check:

Hypothesis 2:
- Cause:
- Supporting evidence:
- Evidence against:
- Missing evidence:
- Confidence:
- Next check:

### Recommendation

State the recommended next action: prove a hypothesis, inspect one specific file/function, ask for reproduction details, recommend implementing a minimal fix in the next step, or stop because scope is expanding.
```
