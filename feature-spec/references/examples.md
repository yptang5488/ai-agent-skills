# Feature Spec Examples

Use these examples only when they clarify how to write or update a feature spec. Keep the main spec focused on stable expected behavior, acceptance criteria, validation contracts, manual verification, and open questions.

## UI-Heavy Mask Dropdown Hover Behavior

Example requirement:

> When hovering any mask entry in the MaskInspector dropdown, the corresponding mask preview should be shown in the viewer regardless of the current active mask tab.

Possible spec breakdown:

```markdown
## Expected Behavior

- When the user hovers any mask entry in the MaskInspector dropdown, the viewer should show the corresponding mask preview.
- The behavior should be consistent across Brush, Gradient, Radial, and Partial Adjust mask tabs.
- Hover preview should not depend on the currently active mask tab.
- The preview should clear when dropdown hover ends.

## Acceptance Criteria

- Hovering a mask entry previews the correct mask in the viewer.
- The same hover behavior works from each supported mask tab.
- Leaving the dropdown clears the hover preview without leaving stale mask info in the viewer.

## Agent-Checkable Validation

- Dropdown hover updates a shared hover target containing mask id and mask type.
- Dropdown leave or clear behavior resets the shared hover target.
- Brush, Gradient, Radial, and Partial Adjust handlers can read the shared hover target.
- Viewer overlay drawing checks the shared hover target instead of relying only on the active tab.
- Cross-type previews are routed through shared overlay logic or an equivalent shared contract.
- Suppression rules such as active drag, capture, or viewer mouse leave are explicit.

## Manual Verification

- In Brush tab, hover Brush / Gradient / Radial / Partial Adjust dropdown entries and confirm the correct viewer preview appears.
- Repeat the same check in Gradient, Radial, and Partial Adjust tabs.
- Move the cursor out of the dropdown and confirm the preview clears.
- Confirm cursor state, selected mask display, redraw timing, and overlay z-order remain correct after hover ends.

## Open Questions

- Should hover preview be suppressed during active drag or capture?
- Which owner has priority if dropdown hover and active tool cursor logic both want to update cursor state?
```

This level of detail is useful for UI behavior that spans multiple owners, event paths, or visual states. Simpler feature specs should stay shorter.

## Cursor Priority And Restoration

Use this pattern when behavior depends on state modified by multiple owners. Define ownership, priority, and restoration rules instead of validating only one code path.

Weak spec wording:

```markdown
- Hover changes the cursor.
```

Stronger spec wording:

```markdown
## Expected Behavior

- Dropdown hover preview should not permanently override cursor state after hover ends.
- Active drag or capture state has higher priority than dropdown hover preview.
- After hover ends, the active viewer handler should restore cursor state based on the current mouse position and active tool state.

## Agent-Checkable Validation

- Cursor ownership or priority rules are explicit where dropdown hover preview and active tool cursor logic can both update cursor state.
- Hover-end handling routes cursor restoration through the active viewer/tool state or an equivalent shared contract.
- Active drag or capture suppression is explicit.

## Manual Verification

- Hover a dropdown entry, then leave the dropdown and confirm the cursor returns to the correct state for the active tool and mouse position.
- Repeat while drag or capture interactions are active, if supported.
```

This level of detail is useful when multiple handlers can modify shared UI state such as cursor, overlay, selection, hover target, or capture state.
