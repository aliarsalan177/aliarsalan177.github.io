# Agent Guide — UI/UX Design

> Hierarchy, spacing, contrast and accessibility — the systematic rules that make interfaces feel obvious.
> Category: Frontend · Source: https://aliarsalan177.github.io/guides/ui-ux/

## Purpose

A field guide for **AI coding agents** (and engineers): the concepts to apply and the
mistakes to avoid when building with UI/UX Design. Load this into your agent's context so it
keeps these concepts in mind and does **not** repeat these mistakes.

## Rules — Apply / Avoid

### 1. Establish a visual hierarchy
- ✅ **APPLY:** Use size, weight, color and spacing so the eye lands on the most important thing first.
- ⛔ **AVOID:** Giving everything equal emphasis — when everything shouts, nothing is heard.

### 2. Space on a consistent scale
- ✅ **APPLY:** Adopt an 8px spacing system and a modular type scale so rhythm stays consistent and predictable.
- ⛔ **AVOID:** Ad-hoc margins/padding (7px here, 13px there) that make the UI feel noisy and unbalanced.

### 3. Accessibility is a requirement, not a checkbox
- ✅ **APPLY:** Meet WCAG contrast (4.5:1 body text), give visible focus states, label controls, and size touch targets ≥44px.
- ⛔ **AVOID:** Low-contrast gray-on-gray, invisible focus rings, and icon-only buttons with no accessible label.

### 4. Systematize with a design system
- ✅ **APPLY:** Build up from tokens (color, type, spacing) to components to patterns so the product stays coherent as it grows.
- ⛔ **AVOID:** One-off components and hard-coded values that drift and multiply inconsistency.

## Cheat Reference — concepts to remember

- **Establish a visual hierarchy** — Use size, weight, color and spacing so the eye lands on the most important thing first.
- **Space on a consistent scale** — Adopt an 8px spacing system and a modular type scale so rhythm stays consistent and predictable.
- **Accessibility is a requirement, not a checkbox** — Meet WCAG contrast (4.5:1 body text), give visible focus states, label controls, and size touch targets ≥44px.
- **Systematize with a design system** — Build up from tokens (color, type, spacing) to components to patterns so the product stays coherent as it grows.

## Full Cheat Sheet — every concept

### Hierarchy & Typography
- Hierarchy via size, weight, color, position, whitespace — guide the eye to what matters.
- Typefaces: serif / sans-serif / display / mono; use a modular type scale (e.g. 1.618).

### Color & Contrast
- Schemes: monochromatic, analogous, complementary, split-complementary, triadic, tetradic.
- Meet WCAG contrast (4.5:1 body text); never rely on color alone.

### Spacing, Grid, Layout
- 8px base unit; increment spacing by it for rhythm.
- 12-column grids: columns, gutters, margins; responsive breakpoints ~600/768/1024/1280.

### Accessibility & Systems
- Visible focus states, adequate touch targets (~44px), labels for screen readers, assistive-tech support.
- Design systems (Atomic Design — Brad Frost): atoms → molecules → organisms → templates → pages.
- Avoid: over-complication, skipping user testing, inconsistent patterns, ignoring a11y.

### Design Process & Deliverables
- Flow: research → sketch → user flows / sitemaps → wireframes → prototypes → test → iterate.
- Levels of fidelity: low (sketch/wireframe) → high (visual/prototype); get feedback early and often.
- Motion & microinteractions to guide attention and give feedback; mobile-first considerations.

## Interview Questions

#### Q1. What is visual hierarchy and how do you create it?
It's the order in which the eye perceives elements. You create it with contrast in size, weight, color, position and whitespace so the most important content is seen first.

#### Q2. How do you ensure a design is accessible?
Meet WCAG color-contrast ratios, provide keyboard focus states, use semantic labels/roles for screen readers, size touch/click targets adequately, and never rely on color alone to convey meaning.

#### Q3. Why use a spacing/grid system?
A base unit (e.g. 8px) and a column grid create consistent rhythm and alignment, make layouts responsive and predictable, and speed up decisions by removing arbitrary values.

---

_Curated by Ali Arsalan · https://aliarsalan177.github.io · Generated from the Engineering Guides._
