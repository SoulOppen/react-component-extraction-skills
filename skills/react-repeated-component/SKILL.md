---
name: react-repeated-component
description: "Trigger: map render, list rendering, .map JSX, repeated items, cards/rows grid. Extract repeated per-item UI into separate component files."
license: Apache-2.0
metadata:
  author: SoulOppen
  version: "1.0"
---

## Activation Contract

Apply this skill when editing or reviewing React/TSX code that:
- renders collections via `.map()`, `.flatMap()`, or equivalent iteration producing JSX;
- repeats a UI block per item (cards, rows, list entries, options, tiles);
- duplicates similar JSX structures representing the same item/entity.

Goal: the per-item UI lives in its own component file; the parent owns iteration and keys.

## Hard Rules

- MUST extract repeated item UI into a separate component file when each iteration renders a meaningful block: ≥2 elements, domain semantics, handlers, conditional bits, or likely to evolve.
- MUST pass item data and callbacks via typed props; the item component MUST NOT own collection-level concerns (fetching the list, pagination, bulk selection state) unless explicitly scoped.
- MUST keep the `key` on the element returned by the parent’s `.map()` (the direct child in the list), using a stable identifier; do not use array index as key when items can reorder, filter, or change, except for static never-reordered lists.
- MUST name files and components after the item/entity (`BookingCard.tsx`, `ServiceRow.tsx`, `TimeSlotOption.tsx`), not generic names like `Item.tsx` unless unavoidable.
- MUST NOT inline non-trivial per-item markup inside `.map()`; extract instead.
- MAY keep a truly trivial, single-primitive iteration inline (e.g. `<li key={id}>{label}</li>` with no logic, handlers, or styling complexity) only if project style allows; prefer extraction when the item has meaning or may grow.
- MUST colocate or place files following existing project conventions.

## Decision Gates

| Situation | Action |
|---|---|
| `.map()` returns multi-element or domain item UI, or item has handlers/conditionals | MUST extract item component to its own file |
| `.map()` returns a single primitive tag with only plain text/value | Prefer extract if meaningful; inline allowed only when trivial and stable |
| Table/list with row actions or per-row state | Extract row/item component; lift shared state via props/callbacks |
| Repeated blocks are static duplicates (not data-driven) | Extract component and render via map over a config/array or reuse |
| Empty/loading/error around the list | Handle list state in parent; apply `react-conditional-component` for those branches |
| Nested lists | Extract inner item component separately when it meets gates |

## Execution Steps

1. Locate repeated JSX (`.map`/duplication) and identify the item boundary and required props.
2. Define the item component API (typed props: item data + callbacks + minimal derived flags) and file path consistent with project conventions.
3. Create the item component file, move per-item markup/logic into it, and export it.
4. Replace the inline body with `items.map(item => <ItemComponent key={item.id} ... />)`; keep iteration, filtering, and empty checks in the parent.
5. Verify types, keys, handlers, and rendering behavior (including empty and single-item cases).

## Output Contract

Return:
- files created/modified (parent and new item component files) with paths;
- item component name, props contract, and key strategy;
- any iteration intentionally left inline with the Decision Gate reason.

## References

- React docs — Rendering Lists: https://react.dev/learn/rendering-lists
- Apply together with `react-conditional-component` for conditional list/container states.
