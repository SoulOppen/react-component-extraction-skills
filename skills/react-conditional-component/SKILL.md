---
name: react-conditional-component
description: "Trigger: conditional render, ternary JSX, && render, loading/error/empty states, gated UI. Extract conditionally rendered UI into separate component files."
license: Apache-2.0
metadata:
  author: SoulOppen
  version: "1.0"
---

## Activation Contract

Apply this skill when editing or reviewing React/TSX code that:
- renders branches with `condition ? <A/> : <B/>`, `condition && <...>`, or `if/else`/`switch` returning different JSX;
- shows mutually exclusive UI states (loading, error, empty, success, forbidden, feature-flagged, role-gated);
- inlines a non-trivial UI block that appears only under a condition.

Goal: each meaningful conditional UI branch lives in its own component file with an intent-based name.

## Hard Rules

- MUST extract a conditional branch to a separate component file when the branch is a distinct UI block: multi-element markup, has its own handlers/hooks/local logic, maps data, or represents a named state/variant.
- MUST extract mutually exclusive view states (loading/error/empty/content, or equivalent) into dedicated components; the parent selects which one to render.
- MUST name files and components after branch intent (`EmptyBookingsState.tsx`, `AdminActions.tsx`, `BookingErrorAlert.tsx`), not after the condition (`IfAdmin.tsx`, `ShowWhenLoading.tsx`).
- MUST keep the condition in the parent (or a selector) and pass only required data/callbacks as typed props; do not hide unrelated branching inside the extracted component.
- MUST preserve behavior, keys, and accessibility; extraction is a refactor unless a behavior change is explicitly requested.
- MUST NOT create a separate file for a purely presentational micro-toggle on the same element when structure stays identical (text/class/attribute/disabled swap).
- MUST NOT duplicate state or fetching logic across branches; lift shared logic to the parent or a hook.
- MUST colocate or place files following existing project conventions; do not invent a new folder structure without need.

## Decision Gates

| Situation | Action |
|---|---|
| `cond && <ExistingComponent/>` or a simple single-element call | Keep inline condition; no new file unless wrapping adds clarity |
| `cond ? <BlockA/> : <BlockB/>` with distinct structure/meaning | Extract `BlockA`/`BlockB` (or each non-trivial side) into separate files |
| Inline branch has ≥2 elements, handlers, hooks, nested conditions, or `.map` | MUST extract to its own file |
| Loading/error/empty/success states in one render | Extract each state view; parent renders the chosen state |
| Same element, only text/class/style/disabled changes | Keep inline or derive props; no extraction |
| Condition selects data/props, not UI structure | Compute values inline; do not extract |
| Repeated items inside a branch | Apply `react-repeated-component` for the item UI |

## Execution Steps

1. Identify conditional JSX and classify each branch (state, variant, or gated section).
2. For each branch meeting a MUST-extract gate, define an intent-based component name, typed props (TypeScript interfaces/types), and a target file path consistent with project conventions.
3. Create the new file, move the branch JSX/logic into it, export the component, and replace the inline branch with `<ExtractedComponent ... />` under the original condition.
4. Update imports and types; run relevant checks to ensure no behavior or accessibility regressions.
5. Re-read the parent render: it should read as a clear selection of states/variants, not embedded branch implementations.

## Output Contract

Return:
- files created/modified (parent and new component files) with paths;
- for each extracted branch: condition/trigger, new component name, and props passed;
- any branch intentionally left inline with the Decision Gate reason.

## References

- React docs — Conditional Rendering: https://react.dev/learn/conditional-rendering
- Apply together with `react-repeated-component` when branches render lists.
