# Agent Guide — SCSS & CSS Architecture

> Sass power features plus a scalable methodology (BEM, 7-1, ITCSS). Structure styles so they stay maintainable at scale — the way design systems like ScandiPWA do.
> Category: Frontend · Source: https://aliarsalan177.github.io/guides/scss/

## Purpose

A field guide for **AI coding agents** (and engineers): the concepts to apply and the
mistakes to avoid when building with SCSS & CSS Architecture. Load this into your agent's context so it
keeps these concepts in mind and does **not** repeat these mistakes.

## Rules — Apply / Avoid

### 1. Name with BEM to avoid collisions
- ✅ **APPLY:** Use Block__Element--Modifier naming so styles are flat, predictable, and scoped by convention.
- ⛔ **AVOID:** Deeply nested selectors and generic names (.title, .active) that leak and fight specificity.

### 2. Keep nesting shallow
- ✅ **APPLY:** Nest at most 2–3 levels; use & for BEM elements/modifiers, not to mirror the DOM.
- ⛔ **AVOID:** 5-level nested selectors that explode specificity and are impossible to override.

### 3. Structure files (7-1 / ITCSS / component-colocated)
- ✅ **APPLY:** Split into abstracts (variables/mixins), base, components, layout, pages; or colocate a .style.scss per component (ScandiPWA style).
- ⛔ **AVOID:** One giant stylesheet, or duplicating variables/mixins across files with no single source of truth.

### 4. Tokenize with variables & maps
- ✅ **APPLY:** Centralize colors, spacing, breakpoints as variables/maps and CSS custom properties; reuse via mixins/functions.
- ⛔ **AVOID:** Magic numbers and hard-coded hex values scattered everywhere.

### 5. Prefer @use over @import
- ✅ **APPLY:** Use the modern module system (@use/@forward) with namespaces; keep partials (_name.scss).
- ⛔ **AVOID:** The deprecated @import (global scope, duplicate output, load-order bugs).

## Cheat Reference — concepts to remember

- **Name with BEM to avoid collisions** — Use Block__Element--Modifier naming so styles are flat, predictable, and scoped by convention.
- **Keep nesting shallow** — Nest at most 2–3 levels; use & for BEM elements/modifiers, not to mirror the DOM.
- **Structure files (7-1 / ITCSS / component-colocated)** — Split into abstracts (variables/mixins), base, components, layout, pages; or colocate a .style.scss per component (ScandiPWA style).
- **Tokenize with variables & maps** — Centralize colors, spacing, breakpoints as variables/maps and CSS custom properties; reuse via mixins/functions.
- **Prefer @use over @import** — Use the modern module system (@use/@forward) with namespaces; keep partials (_name.scss).

## Full Cheat Sheet — every concept

### Sass Features
- Variables ($x), maps, nesting, partials (_file.scss), @use/@forward (not @import).
- @mixin/@include, @function/@return, %placeholder/@extend.
- @if/@each/@for, math, color functions; interpolation #{$x}.

### BEM Naming
- Block (.Card), Element (.Card__title), Modifier (.Card--featured / .Card__title--large).
- Flat, low-specificity selectors; & to build element/modifier names.

### Architecture Patterns
- 7-1: abstracts, base, components, layout, pages, themes, vendors.
- ITCSS: layers by specificity (settings → tools → generic → elements → objects → components → utilities).
- Component-colocated (ScandiPWA): Component.style.scss beside Component; one block per component.

### Tokens & Theming
- Centralize colors/spacing/breakpoints as variables/maps + CSS custom properties for runtime theming.
- Responsive mixins for breakpoints; spacing scale (e.g. 8px) not magic numbers.

## Interview Questions

#### Q1. What is BEM and why use it?
Block__Element--Modifier is a naming methodology that keeps selectors flat and self-documenting, avoiding specificity wars and name collisions. .Button, .Button__icon, .Button--primary make ownership and intent obvious.

#### Q2. Why keep SCSS nesting shallow?
Deep nesting produces long, highly specific selectors that are hard to override and tightly couple CSS to DOM structure. Two-to-three levels (mostly for BEM elements/modifiers via &) keeps specificity low and styles reusable.

#### Q3. @use vs @import in Sass?
@import is deprecated: it dumps everything into the global scope and can duplicate output. @use loads a module once with a namespace (and @forward re-exports), giving encapsulation and predictable load order.

#### Q4. How do you structure SCSS for a large app?
Either a global architecture (7-1: abstracts/base/components/layout/pages/themes/vendors, or ITCSS by specificity) or component-colocated styles (a Component.style.scss next to each component, as ScandiPWA does) with shared tokens/mixins in abstracts.

#### Q5. SCSS mixins vs functions vs placeholders?
Mixins (@mixin/@include) output reusable declaration blocks (can take args); functions (@function/@return) compute values; placeholders (%name/@extend) share rules without output until extended. Prefer mixins over @extend to avoid selector-grouping surprises.

---

_Curated by Ali Arsalan · https://aliarsalan177.github.io · Generated from the Engineering Guides._
