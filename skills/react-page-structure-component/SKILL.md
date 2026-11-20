---
name: react-page-structure-component
description: "Trigger: page header, footer, dialog, section, form extraction. Use framework colocation paths first, else page-scoped files, unless reused across pages."
license: Apache-2.0
metadata:
  author: SoulOppen
  version: "1.0"
---

## Activation Contract
Apply when creating or editing a React/TSX page that contains inline `<header>`, `<footer>`, Dialog/dialog, `<section>`, or `<form>` blocks, or when reviewing such a page.

## Hard Rules
- MUST move every matching page-level block into its own component file before finishing the page.
- Follow the host framework's colocation convention when one exists (e.g., Next.js App Router `app/<route>/_components/`); use the page-scoped folder only as fallback.
- As fallback, place page-only parts in a sibling subfolder named after the page: `pages/welcome/header.tsx`, `pages/auth/register/form.tsx`.
- Move a part to shared `components/` only when the same part is rendered by 2+ pages; repetition inside one page does not count.
- Keep conditions, iteration, state, handlers, and data fetching in the page; pass typed props and callbacks to extracted parts.
- Name files after the part and its intent (`features-section.tsx`, `cancel-booking-dialog.tsx`); use generic `header.tsx`/`footer.tsx`/`form.tsx` when there is only one.
- Do not duplicate markup across parts; reuse the extracted component instead.

## Decision Gates
| Situation | Action |
|---|---|
| Framework defines a colocation path | Use that path; do not invent a new folder |
| Part used by one page only | Extract to framework colocation path, else `pages/<page>/<part>.tsx` |
| Same part used by 2+ pages | Extract to shared `components/` |
| Primitive UI-kit element (Button, Input, Card) | Keep in `components/ui/`; this skill does not apply |
| Several `<section>` blocks | One component per section, intent-named |
| Reuse count unknown | Search all pages before choosing the folder |

## Execution Steps
1. Locate every header/footer/dialog/section/form block in the target page.
2. Detect the framework colocation convention, search other pages for the same part, and choose a framework, fallback, or shared path.
3. Create each file, move the markup, export the component, and pass typed props from the page.
4. Update imports; run format, lint, typecheck, and the relevant tests/build.
5. Return the output contract below.

## Output Contract
Return files created/modified with paths; for each part, its target path and framework/fallback/shared reason; any part left inline with the exact reuse reason; checks run.

## References
- [react-conditional-component](../react-conditional-component/SKILL.md)
- [react-repeated-component](../react-repeated-component/SKILL.md)
