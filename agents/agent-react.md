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

## Full Cheat Sheet — every concept

### JSX
- HTML-like syntax in JS; must return a single parent element (or Fragment <>…</>).
- Attributes are camelCase; use className (not class) and htmlFor (not for).
- Embed JS expressions with { } ; only expressions, not statements.

### Components & Props
- Components are capitalized functions returning JSX; functional components are preferred over class.
- Props pass data parent→child; they're read-only (immutable). Destructure for clarity.
- children is a special prop for content between a component's tags.

### State — useState
- const [state, setState] = useState(initial); changing state triggers a re-render.
- Updates are asynchronous and batched; use the functional form setState(prev => …) when the next value depends on the previous.
- Never mutate state; always set a new object/array.

### Effects & Lifecycle
- useEffect(fn, deps) runs after render for side effects (fetch, subscriptions, DOM).
- Deps: [] = run once on mount; [a,b] = run when a or b change; omitted = every render.
- Return a cleanup function (unsubscribe, clearTimeout) that runs before re-run and on unmount.
- Class lifecycle equivalents: componentDidMount / componentDidUpdate / componentWillUnmount.

### Core Hooks
- useContext(Context) — read a context value (avoids prop drilling).
- useRef(initial) — mutable box (.current) that persists across renders without causing re-renders; also for DOM refs.
- useReducer(reducer, init) — for complex/related state transitions.
- useMemo(fn, deps) — cache an expensive computed value; useCallback(fn, deps) — cache a function reference.
- Custom hooks — reuse stateful logic; name them use…

### Rules of Hooks
- Only call hooks at the top level — never in conditions, loops, or nested functions.
- Only call hooks from React function components or other hooks.
- This keeps hook call order stable across renders.

### Lists & Keys
- Render lists with .map(); each element needs a stable, unique key.
- Key from data id — never the array index for reorderable/filterable lists.
- Keys help React diff and preserve component state correctly.

### Performance
- React.memo(Component) — skip re-render when props are shallowly equal.
- useMemo / useCallback — keep values/functions referentially stable for memoized children and effect deps.
- Optimize intentionally — measure first; needless memoization adds overhead.

### Forms & Context
- Controlled components: value + onChange bind an input to state (single source of truth).
- Context: createContext → <Provider value> → useContext to consume; good for themes/auth/locale.
- Don't overuse context for frequently-changing values (causes wide re-renders).

### Elements & Rendering
- Fragments (<>…</> or <React.Fragment>) return siblings without an extra DOM node.
- Conditional rendering: {cond && <A/>}, ternary {cond ? <A/> : <B/>}, or early return.
- Inline styles are objects (style={{color:'red'}}); embedded JS in { }.
- Error boundaries (class componentDidCatch / getDerivedStateFromError) catch render errors in the subtree.

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
