# Agent Guide — JavaScript (Advanced Concepts)

> The engine, closures, prototypes, the event loop and coercion — the fundamentals that separate framework authors from framework users.
> Category: Languages · Source: https://aliarsalan177.github.io/guides/javascript/

## Purpose

A field guide for **AI coding agents** (and engineers): the concepts to apply and the
mistakes to avoid when building with JavaScript (Advanced Concepts). Load this into your agent's context so it
keeps these concepts in mind and does **not** repeat these mistakes.

## Rules — Apply / Avoid

### 1. Closures are the core primitive
- ✅ **APPLY:** Use closures deliberately for encapsulation and private state — a function keeps access to its outer scope even after that scope returns.
- ⛔ **AVOID:** Capturing large objects (or DOM nodes) in a long-lived closure — they can't be garbage-collected and become silent memory leaks.

### 2. The event loop: micro vs macro tasks
- ✅ **APPLY:** Remember Promise/microtask callbacks drain fully before the next macrotask (setTimeout). Order your async work with that priority in mind.
- ⛔ **AVOID:** Assuming setTimeout(fn, 0) runs before a resolved Promise — it never does; the microtask queue always wins.

### 3. `this` is bound by call-site, not definition
- ✅ **APPLY:** Reason about `this` by how a function is called (new / implicit / explicit / arrow). Use arrow functions to lexically inherit `this` in callbacks.
- ⛔ **AVOID:** Passing an object method as a bare callback and losing `this` — bind it, wrap it, or make it an arrow.

### 4. Objects are passed by reference
- ✅ **APPLY:** Clone before mutating shared state. Spread/Object.assign copy one level; use structuredClone for deep copies.
- ⛔ **AVOID:** Shallow-copying a nested object and then mutating a nested field — you've just mutated the original too.

### 5. Prototypes power the object model
- ✅ **APPLY:** Add shared methods to the prototype (or class body) so instances share one function, not a fresh copy each.
- ⛔ **AVOID:** Using `delete` on hot objects or setting properties in inconsistent order — it deopts the engine's hidden classes.

### 6. Always use strict equality
- ✅ **APPLY:** Compare with === / !== and convert types explicitly when you mean to.
- ⛔ **AVOID:** Relying on == coercion (`0 == ''`, `null == undefined`) — it hides bugs that only surface with odd inputs.

## Cheat Reference — concepts to remember

- **Closures are the core primitive** — Use closures deliberately for encapsulation and private state — a function keeps access to its outer scope even after that scope returns.
- **The event loop: micro vs macro tasks** — Remember Promise/microtask callbacks drain fully before the next macrotask (setTimeout). Order your async work with that priority in mind.
- **`this` is bound by call-site, not definition** — Reason about `this` by how a function is called (new / implicit / explicit / arrow). Use arrow functions to lexically inherit `this` in callbacks.
- **Objects are passed by reference** — Clone before mutating shared state. Spread/Object.assign copy one level; use structuredClone for deep copies.
- **Prototypes power the object model** — Add shared methods to the prototype (or class body) so instances share one function, not a fresh copy each.
- **Always use strict equality** — Compare with === / !== and convert types explicitly when you mean to.

## Interview Questions

#### Q1. What is a closure and why is it useful?
A closure is a function bundled with references to its surrounding lexical scope, so it can access those variables even after the outer function has returned. It powers data privacy/encapsulation, function factories, and stable references in async code.

#### Q2. Explain the event loop, task queue and microtask queue.
JS is single-threaded; the event loop moves queued callbacks onto the empty call stack. Microtasks (Promises, queueMicrotask) have priority and are drained completely before the next macrotask (setTimeout, I/O, UI events).

#### Q3. How does `this` get its value?
By call-site: `new` binds it to the new object, explicit binding (call/apply/bind) sets it, implicit binding uses the object left of the dot, otherwise it's undefined (strict) or the global object. Arrow functions have no own `this` — they inherit it lexically.

#### Q4. Difference between `var`, `let`, and `const`?
`var` is function-scoped and hoisted as undefined; `let`/`const` are block-scoped and live in a temporal dead zone until declared. `const` prevents reassignment (not deep immutability).

#### Q5. Shallow vs deep copy?
A shallow copy (spread, Object.assign) duplicates only the top level; nested objects are still shared references. A deep copy (structuredClone, or a recursive clone) duplicates the whole tree so mutations don't leak.

#### Q6. What is the prototype chain?
Every object has an internal link (__proto__) to another object; property lookups walk up this chain until found or null. It's how inheritance and shared methods work in JavaScript.

---

_Curated by Ali Arsalan · https://aliarsalan177.github.io · Generated from the Engineering Guides._
