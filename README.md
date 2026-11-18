# React Component Extraction Skills

Complementary agent skills that enforce React component extraction patterns so UI code stays clear, reusable, and easy to review.

These skills target two common cases that should not stay inlined:
- **Conditional UI**: when a branch renders only under a condition (states, variants, or gated sections), extract it into its own component file.
- **Repeated UI**: when the same UI structure renders multiple times (lists, maps, grids, rows, or cards), extract the item into its own component file.

## Skills

| Skill | Purpose | Typical triggers |
| --- | --- | --- |
| `react-conditional-component` | Extract conditionally rendered UI branches into separate component files (one meaningful branch/state per component). | `condition ? A : B`, `condition && <...>`, loading/error/empty/success states, permission/feature-flag gated UI |
| `react-repeated-component` | Extract repeated per-item UI into separate component files while keeping iteration and keys in the parent. | `.map()` / `.flatMap()` JSX, cards/lists/rows/options rendered per item, duplicated item markup |

## Installation

Install with the [skills CLI](https://github.com/vercel-labs/skills) (`npx skills`):

```bash
# Install both skills (project scope by default)
npx skills add SoulOppen/react-component-extraction-skills

# Install a single skill
npx skills add SoulOppen/react-component-extraction-skills@react-conditional-component
npx skills add SoulOppen/react-component-extraction-skills@react-repeated-component
```

- Add `-g` to install globally, or `-y` to skip prompts.
- Run from the project where you want the skills available.
- See `npx skills --help` for agent and scope options.

## When to use

- A render branch is a distinct UI block (multi-element, has handlers/hooks, maps data, or represents a named state/variant) → extract it (`react-conditional-component`).
- A `.map()` (or similar) returns meaningful per-item UI → extract the item component and keep the `key` on the mapped element in the parent (`react-repeated-component`).
- Keep trivial, same-structure micro-toggles (text/class/attribute only) inline when extraction adds no clarity.

The two skills are complementary: conditional list states (loading/empty/error/content) use `react-conditional-component`, and the rendered items use `react-repeated-component`.

## Repository structure

```text
skills/
├── react-conditional-component/
│   └── SKILL.md
└── react-repeated-component/
    └── SKILL.md
```

Each `SKILL.md` defines activation triggers, hard rules, decision gates, execution steps, and an output contract.

## Use as a Git submodule

To keep these skills versioned and updatable inside another repository:

```bash
# Add as a submodule (choose any path you prefer)
git submodule add https://github.com/SoulOppen/react-component-extraction-skills.git skills/external/react-component-extraction-skills

# Update to the latest version later
git submodule update --remote --merge skills/external/react-component-extraction-skills
```

Commit the `.gitmodules` entry and the submodule pointer in the consuming repository after adding or updating.

## Compatibility

- Designed for React/TSX codebases (including React + TypeScript stacks such as Inertia + component libraries).
- Distributed in the standard agent-skills format (`skills/<name>/SKILL.md`) compatible with `npx skills add`.

## License

Apache-2.0. See [LICENSE](LICENSE).
