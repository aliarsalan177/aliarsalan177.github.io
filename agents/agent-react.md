# Agent Guide — React

> Declarative UI from state. Master hooks, keys, memoization and effect discipline to build fast, predictable interfaces.
> Category: Frontend · Source: https://aliarsalan177.github.io/guides/react/

## Purpose

A field guide for **AI coding agents** (and engineers): the concepts to apply and the
mistakes to avoid when building with React. Load this into your agent's context so it
keeps these concepts in mind and does **not** repeat these mistakes.

## Rules — Apply / Avoid

### 1. State is immutable — replace, don't mutate
- ✅ **APPLY:** Return new objects/arrays from setState. Derive rendered values from state during render.
- ⛔ **AVOID:** Mutating state in place (push, splice, obj.x = …) — React won't re-render and you get stale UI.

### 2. Rules of Hooks
- ✅ **APPLY:** Call hooks unconditionally at the top level of a component or custom hook, in the same order every render.
- ⛔ **AVOID:** Calling hooks inside conditions, loops, or nested functions — it breaks the hook order and crashes.

### 3. useEffect is for synchronizing with the outside world
- ✅ **APPLY:** Use effects for subscriptions, network, and DOM side effects; return a cleanup; list every dependency.
- ⛔ **AVOID:** Using effects to compute derived state (do it during render) or omitting deps to 'fix' loops — that hides bugs.

### 4. Stable, unique keys in lists
- ✅ **APPLY:** Key list items by a stable id from your data so React can diff correctly.
- ⛔ **AVOID:** Using the array index as a key for reorderable/filterable lists — it corrupts state and inputs on reorder.

### 5. Memoize intentionally, not reflexively
- ✅ **APPLY:** Reach for React.memo / useMemo / useCallback when profiling shows a real cost or to keep referential stability for deps.
- ⛔ **AVOID:** Wrapping everything in useMemo/useCallback by default — the bookkeeping can cost more than it saves.

## Cheat Reference — concepts to remember

- **State is immutable — replace, don't mutate** — Return new objects/arrays from setState. Derive rendered values from state during render.
- **Rules of Hooks** — Call hooks unconditionally at the top level of a component or custom hook, in the same order every render.
- **useEffect is for synchronizing with the outside world** — Use effects for subscriptions, network, and DOM side effects; return a cleanup; list every dependency.
- **Stable, unique keys in lists** — Key list items by a stable id from your data so React can diff correctly.
- **Memoize intentionally, not reflexively** — Reach for React.memo / useMemo / useCallback when profiling shows a real cost or to keep referential stability for deps.

## Interview Questions

#### Q1. Why can't you mutate state directly?
React decides to re-render by comparing references. Mutating in place keeps the same reference, so the update is missed and the UI goes stale. Always produce a new object/array via setState.

#### Q2. What are the rules of hooks?
Only call hooks at the top level (never in conditions, loops, or nested functions) and only from React functions. This keeps the call order stable so React can associate state with each hook.

#### Q3. useMemo vs useCallback vs React.memo?
useMemo caches a computed value, useCallback caches a function reference, and React.memo skips re-rendering a component when its props are shallowly equal. All are optimizations — measure first.

#### Q4. Why do lists need keys, and why not the index?
Keys let React match elements between renders to preserve state and minimize DOM work. Indexes break when the list reorders/filters, causing wrong state to stick to the wrong item.

#### Q5. When does useEffect run and how do you clean up?
After render/paint, and again whenever a dependency changes; the empty array runs once on mount. Returning a function from the effect cleans up (unsubscribe, clear timers) before the next run and on unmount.

---

_Curated by Ali Arsalan · https://aliarsalan177.github.io · Generated from the Engineering Guides._
