# Agent Guide — Tailwind CSS

> Utility-first CSS that scales via constraints. Compose utilities, extract components, and theme with tokens instead of writing custom CSS.
> Category: Frontend · Source: https://aliarsalan177.github.io/guides/tailwind/

## Purpose

A field guide for **AI coding agents** (and engineers): the concepts to apply and the
mistakes to avoid when building with Tailwind CSS. Load this into your agent's context so it
keeps these concepts in mind and does **not** repeat these mistakes.

## Rules — Apply / Avoid

### 1. Utility-first, extract when repeated
- ✅ **APPLY:** Compose utilities in markup; when a pattern repeats, extract a component (React) or use @apply sparingly.
- ⛔ **AVOID:** Recreating a full custom CSS layer, or wrapping every element in @apply (you lose the utility benefits).

### 2. Theme through config/tokens
- ✅ **APPLY:** Define colors, spacing, fonts in the theme (tailwind.config or @theme in v4) and use semantic tokens.
- ⛔ **AVOID:** Hard-coded arbitrary values everywhere ([#3b82f6], [13px]) that bypass your design system.

### 3. Compose classes safely
- ✅ **APPLY:** Merge conditional classes with clsx + tailwind-merge (cn helper) so later utilities win predictably.
- ⛔ **AVOID:** String-concatenating classes that produce conflicts (p-2 and p-4 both present) with no resolution.

### 4. Use state & responsive variants
- ✅ **APPLY:** Prefix with responsive (md:) and state (hover:, focus:, dark:, group-hover:) variants instead of custom CSS.
- ⛔ **AVOID:** Writing media queries and pseudo-class CSS by hand when a variant exists.

### 5. Rely on JIT purging
- ✅ **APPLY:** Ensure content paths are configured so unused classes are purged; keep class names statically analyzable.
- ⛔ **AVOID:** Building class names dynamically (`text-${color}-500`) — the purge can't see them and they vanish in prod.

## Cheat Reference — concepts to remember

- **Utility-first, extract when repeated** — Compose utilities in markup; when a pattern repeats, extract a component (React) or use @apply sparingly.
- **Theme through config/tokens** — Define colors, spacing, fonts in the theme (tailwind.config or @theme in v4) and use semantic tokens.
- **Compose classes safely** — Merge conditional classes with clsx + tailwind-merge (cn helper) so later utilities win predictably.
- **Use state & responsive variants** — Prefix with responsive (md:) and state (hover:, focus:, dark:, group-hover:) variants instead of custom CSS.
- **Rely on JIT purging** — Ensure content paths are configured so unused classes are purged; keep class names statically analyzable.

## Full Cheat Sheet — every concept

### Core Idea
- Utility-first: compose classes (flex, p-4, text-lg, bg-accent) in markup.
- Constraints from the theme give consistency; JIT purges unused classes.

### Layout & Spacing
- Flex/grid (flex, grid, gap-4), spacing scale (p-*, m-*, space-x-*), sizing (w-*, h-*, max-w-*).
- Position, z-index, overflow utilities.

### Variants
- Responsive: sm: md: lg: xl: 2xl: (mobile-first).
- State: hover: focus: active: disabled: dark: group-hover: peer-*.
- Arbitrary values [13px] and arbitrary properties as escape hatches (sparingly).

### Theming & Tooling (v4)
- v4: @import "tailwindcss" + @theme tokens (CSS-first config); v3: tailwind.config.js theme.extend.
- Compose classes with clsx + tailwind-merge (cn helper).
- @layer/@apply for small extractions; plugins (typography, forms) for extras.

## Interview Questions

#### Q1. What does 'utility-first' mean and what's the trade-off?
You style by composing small single-purpose classes in markup instead of writing custom CSS. Benefits: speed, consistency via constraints, no naming/specificity battles, tiny purged CSS. Trade-off: verbose markup — mitigated by extracting components.

#### Q2. How do you avoid repeating utility strings?
Extract reusable components (the primary approach in React), use @apply for small shared patterns, or compose with a cn() helper. Design tokens in the theme keep values consistent.

#### Q3. Why use tailwind-merge with clsx?
clsx builds conditional class strings, but conflicting utilities (p-2 vs p-4) both remain. tailwind-merge resolves conflicts so the last one wins — the common cn() helper composes both.

#### Q4. How does Tailwind keep the CSS bundle small?
The JIT engine scans your content files and generates only the classes you actually use, purging the rest. This requires class names to be complete strings it can detect — not dynamically constructed.

#### Q5. Tailwind vs traditional CSS/SCSS?
Tailwind trades semantic class names for utility composition and a constrained design system, reducing custom CSS and naming overhead. SCSS/BEM gives semantic, centralized styles better for complex bespoke design. Many teams combine: Tailwind for app UI, custom CSS for edge cases.

---

_Curated by Ali Arsalan · https://aliarsalan177.github.io · Generated from the Engineering Guides._
