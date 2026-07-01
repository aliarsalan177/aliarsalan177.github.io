# AGENTS — Engineering Guides (All Topics)

> 21 topics · Curated by Ali Arsalan · https://aliarsalan177.github.io/guides/

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

### 7. Write engine-friendly (optimizable) code
- ✅ **APPLY:** Keep object shapes consistent (init properties in the same order) so V8 keeps a shared hidden class and inline-caches hot calls.
- ⛔ **AVOID:** eval, with, and `delete` on hot objects, plus adding properties in varying orders — each deopts the engine.

### 8. Memoize pure, expensive functions
- ✅ **APPLY:** Cache results by argument (a closure over a cache object) so repeat calls with the same input are instant.
- ⛔ **AVOID:** Memoizing impure functions or unbounded caches — you cache wrong answers and leak memory.

### 9. Mind the call stack and recursion depth
- ✅ **APPLY:** Bound recursion or convert deep recursion to iteration; heavy work belongs off the main thread (Web Workers).
- ⛔ **AVOID:** Unbounded recursion ('Maximum call stack size exceeded') and long synchronous work that freezes the UI.

### 10. Prevent memory leaks
- ✅ **APPLY:** Remove event listeners, clear timers/intervals, and drop references you no longer need so mark-and-sweep can collect.
- ⛔ **AVOID:** Lingering globals, detached DOM nodes held by closures, and forgotten listeners — the classic leak sources.

### 11. Favor immutability & pure functions
- ✅ **APPLY:** Return new data instead of mutating; keep functions side-effect-free (same input → same output) and compose them (pipe/compose).
- ⛔ **AVOID:** Shared mutable state and side effects scattered through the code — they make behavior unpredictable and untestable.

### 12. Prefer composition over inheritance
- ✅ **APPLY:** Compose small behaviors onto objects; use ES modules (import/export) to structure code and avoid global scope.
- ⛔ **AVOID:** Deep inheritance hierarchies (tight coupling, fragile base class) and leaking everything onto the global namespace.

### 13. Handle async errors explicitly
- ✅ **APPLY:** Wrap awaited calls in try/catch and attach .catch to promises; know your error types (TypeError, RangeError, …).
- ⛔ **AVOID:** Unhandled promise rejections — async errors fail silently and leave the app in a broken state.

## Cheat Reference — concepts to remember

- **Closures are the core primitive** — Use closures deliberately for encapsulation and private state — a function keeps access to its outer scope even after that scope returns.
- **The event loop: micro vs macro tasks** — Remember Promise/microtask callbacks drain fully before the next macrotask (setTimeout). Order your async work with that priority in mind.
- **`this` is bound by call-site, not definition** — Reason about `this` by how a function is called (new / implicit / explicit / arrow). Use arrow functions to lexically inherit `this` in callbacks.
- **Objects are passed by reference** — Clone before mutating shared state. Spread/Object.assign copy one level; use structuredClone for deep copies.
- **Prototypes power the object model** — Add shared methods to the prototype (or class body) so instances share one function, not a fresh copy each.
- **Always use strict equality** — Compare with === / !== and convert types explicitly when you mean to.
- **Write engine-friendly (optimizable) code** — Keep object shapes consistent (init properties in the same order) so V8 keeps a shared hidden class and inline-caches hot calls.
- **Memoize pure, expensive functions** — Cache results by argument (a closure over a cache object) so repeat calls with the same input are instant.
- **Mind the call stack and recursion depth** — Bound recursion or convert deep recursion to iteration; heavy work belongs off the main thread (Web Workers).
- **Prevent memory leaks** — Remove event listeners, clear timers/intervals, and drop references you no longer need so mark-and-sweep can collect.
- **Favor immutability & pure functions** — Return new data instead of mutating; keep functions side-effect-free (same input → same output) and compose them (pipe/compose).
- **Prefer composition over inheritance** — Compose small behaviors onto objects; use ES modules (import/export) to structure code and avoid global scope.
- **Handle async errors explicitly** — Wrap awaited calls in try/catch and attach .catch to promises; know your error types (TypeError, RangeError, …).

## Full Cheat Sheet — every concept

### JavaScript Engine
- Engine = program that executes JS (V8 for Chrome/Node, SpiderMonkey for Firefox); all standardized by ECMAScript (ES).
- Pipeline: Parser → AST (Abstract Syntax Tree) → Interpreter → bytecode; a Profiler watches hot code.
- Compiler converts hot code ahead of time to optimized machine code (Babel & TypeScript are compilers too).
- The Combo = JIT (Just-In-Time): interpreter for fast start + compiler for optimized hot paths. V8 pioneered it (2008, C++).

### Writing Optimized Code
- Memoization: cache a function's return value by its arguments (via a closure) so repeat calls are instant.
- Inline caching: repeated calls with the same object shape get optimized; varying shapes deopt it.
- Hidden classes: initialize object properties in the same order across instances; avoid `delete` (it changes the hidden class).
- Managing arguments: safe to use arguments.length, arguments[i], and fn.apply(y, arguments); never use `arguments` bare.
- Avoid where possible: eval(), arguments (bare), for-in, with, delete — they hinder the compiler.

### Call Stack & Memory Heap
- Memory heap: unordered storage for objects and data.
- Call stack: LIFO structure tracking execution; each call pushes a stack frame.
- Stack overflow: infinite/too-deep recursion → 'Maximum call stack size exceeded'.
- Garbage collection: automatic mark-and-sweep frees unreachable memory; leaks come from lingering references.

### Event Loop & Async
- JS is single-threaded; the browser/Node provide Web APIs that run off-thread.
- Event loop moves queued callbacks onto the empty call stack.
- Callback (task) queue = setTimeout, events; Job (microtask) queue = Promises, and has higher priority.
- Microtasks drain fully before the next macrotask — a resolved Promise runs before setTimeout(…, 0).
- 3 ways to Promise: Promise.all (parallel), await each (sequential), Promise.race (first to settle).
- Threads/concurrency/parallelism: Web Workers run background threads and communicate by message passing.

### Execution Context
- Global EC: creates the global object and `this`; hoists vars as undefined and functions fully.
- Function EC: created per call; sets up the `arguments` object and `this` by call-site.
- Arrow functions: no own `this`, `arguments`, `super`, or `new.target`; inherit `this` lexically; can't be constructors.

### Hoisting
- Function declarations are fully hoisted (callable before definition).
- `var` is hoisted and initialized to undefined; `let`/`const` are hoisted but in the Temporal Dead Zone until declared.
- Best practice: prefer let/const and declare before use.

### Scope & Lexical Environment
- Lexical environment = where code is physically written; new scope per {} block for let/const.
- Scope chain: inner scopes can read outer variables, walking up until found — not the reverse.
- Function scope (var) vs block scope (let/const); `var` leaks out of loops/blocks.
- IIFE `(function(){})()` creates a private scope to avoid polluting the global namespace.

### The `this` Keyword
- It matters HOW a function is called, not where it's written.
- 4 binding rules: new binding (new), implicit (obj.method()), explicit (call/apply/bind), arrow (lexical).
- Nested regular functions default `this` to the global/undefined; arrows inherit the enclosing `this`.
- Context = value of `this`; Scope = variable visibility. Lexical (JS default) vs dynamic scope.

### Call / Apply / Bind
- call(thisArg, arg1, arg2) — invoke now, borrow a method for another object.
- apply(thisArg, [args]) — same as call but arguments as an array.
- bind(thisArg, ...preset) — returns a new function (deferred), enabling currying / partial application.

### JavaScript Types
- 7 primitives: string, number, bigint, boolean, null, undefined, symbol — immutable, passed & compared by value.
- Non-primitives (objects/arrays/functions): mutable, passed & compared by reference.
- Gotcha: typeof null === 'object' (legacy bug).
- Shallow copy: spread/Object.assign (top level only); deep copy: structuredClone / JSON round-trip.
- == coerces types; === does not — always prefer ===.
- Static vs dynamic typing (JS is dynamic); weakly vs strongly typed (JS is weakly typed).

### The 2 Pillars — Closures
- Closure: a function retains access to its outer (lexical) scope even after that scope returns.
- Uses: memory efficiency (reuse scope), encapsulation (private state), and function factories.

### The 2 Pillars — Prototypes & OOP building blocks
- Prototypal inheritance: objects inherit via the prototype chain; all descend from Object.
- `prototype` (function property to add shared methods) vs `__proto__` (the link between instances).
- Constructor functions: capitalized, called with `new`, use `this`; without `new`, `this` is global.
- Classes (ES6): syntactic sugar over prototypes; not hoisted; support extends/super/instanceof and #private fields.
- Object.create makes an object with a given prototype; factory functions return objects; method stores share methods.

### 4 Pillars of OOP
- Encapsulation — bundle data with methods and hide internals.
- Abstraction — expose only what's necessary, hide complexity.
- Inheritance — derive new types from existing ones.
- Polymorphism — one interface, many implementations.

### Functional Programming
- Pure functions: no side effects, same input → same output; do one thing.
- Referential transparency: a call can be replaced by its result; Idempotence: repeating yields the same result.
- Imperative (how) vs Declarative (what) — shift toward declarative (map/forEach over manual loops).
- Immutability: don't mutate; copy. Structural sharing reuses unchanged parts.
- Partial application & currying; Pipe (left→right) and Compose (right→left); Arity: keep to 1–2 args.
- Higher-order functions take/return functions to reduce repetition.

### Composition vs Inheritance
- Composition (FP): flexible, combine behaviors — generally preferred.
- Inheritance (OOP): rigid; problems are tight coupling, fragile base class, and inheriting unwanted methods.
- React uses both: class components (OOP) + pure function components (FP).

### Modules
- Module pattern: IIFE returning an object that exposes a public interface, hiding privates.
- CommonJS: require() / module.exports — synchronous, used by Node (bundlers for the browser).
- AMD: define() — asynchronous loading (RequireJS).
- ES6 modules: import / export (named & default); need type="module" and to be served over http.

### Error Handling
- Synchronous: try / catch / finally.
- Asynchronous: .catch() on promises or try/catch with async-await; unhandled async errors fail silently.
- 8 error types: Error, EvalError, InternalError, RangeError, ReferenceError, SyntaxError, TypeError, URIError.
- Error properties: name, message, stack. Custom errors extend the Error class.

### Data Structures (bonus)
- Arrays: contiguous memory; O(1) index access, O(n) search/insert/delete in the middle.
- Hash tables: key→value with a hash function; O(1) average lookup; handle collisions (chaining/probing).
- Hashing in JS underlies objects and Maps; collisions degrade performance toward O(n).

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

#### Q7. How does the JavaScript engine run your code?
The parser produces an AST; the interpreter turns it into bytecode and runs it, while a profiler flags hot code that the compiler optimizes to machine code. This interpreter+compiler mix is JIT ('Just-In-Time'), pioneered by V8.

#### Q8. What are hidden classes and inline caching?
V8 optimizations: hidden classes let objects with the same shape share an internal structure, and inline caching speeds up repeated property access on that shape. Adding properties in different orders or using `delete` breaks the shared hidden class and deoptimizes.

#### Q9. How does garbage collection work, and how do leaks happen?
V8 uses mark-and-sweep: it marks reachable objects from roots and frees the rest. Leaks occur when references stay reachable unintentionally — globals, uncleared timers/listeners, and detached DOM held by closures.

#### Q10. Explain execution context and hoisting.
Each run creates an execution context with a creation phase (set up `this`, hoist declarations — var as undefined, functions fully, let/const in the temporal dead zone) and an execution phase. That creation-phase setup is why hoisting exists.

#### Q11. What is an IIFE and why use it?
An Immediately Invoked Function Expression runs as soon as it's defined `(function(){...})()`. It creates a private scope so variables don't pollute the global namespace — the basis of the classic module pattern.

#### Q12. call vs apply vs bind?
call(thisArg, ...args) and apply(thisArg, [args]) invoke immediately (differing only in how args are passed); bind(thisArg, ...args) returns a new function with `this` and optional preset arguments (partial application / currying).

#### Q13. What is currying / partial application?
Transforming a multi-arg function into a chain of single-arg functions, or pre-filling some arguments (often via bind) to produce a specialized function for later use.

#### Q14. Pure functions, referential transparency and idempotence?
A pure function has no side effects and returns the same output for the same input. Referential transparency means a call can be replaced by its result without changing behavior; idempotence means repeating the operation yields the same result.

#### Q15. Composition vs inheritance — and what are OOP's problems?
Composition builds behavior by combining small pieces (flexible, has-a); inheritance shares state down a hierarchy (rigid, is-a). Classic OOP pitfalls are tight coupling, the fragile base class, and inheriting unwanted methods.

#### Q16. Module systems: module pattern vs CommonJS vs ES6 modules?
The module pattern uses an IIFE returning a public object. CommonJS (require/module.exports) is synchronous and Node's classic system. ES6 modules (import/export) are static, support named/default exports, and are the modern standard.

#### Q17. How do you handle synchronous vs asynchronous errors?
Synchronous: try/catch/finally. Asynchronous: .catch() on promises or try/catch around await — otherwise async errors fail silently. Error objects carry name, message and stack; you can extend Error for custom types.

---

_Curated by Ali Arsalan · https://aliarsalan177.github.io · Generated from the Engineering Guides._


<hr>

# Agent Guide — TypeScript

> A structural type system over JavaScript — model your data precisely, let inference work, and make illegal states unrepresentable.
> Category: Languages · Source: https://aliarsalan177.github.io/guides/typescript/

## Purpose

A field guide for **AI coding agents** (and engineers): the concepts to apply and the
mistakes to avoid when building with TypeScript. Load this into your agent's context so it
keeps these concepts in mind and does **not** repeat these mistakes.

## Rules — Apply / Avoid

### 1. Prefer `unknown` over `any`
- ✅ **APPLY:** Accept `unknown` at boundaries and narrow with type guards before use — you keep type safety.
- ⛔ **AVOID:** Sprinkling `any` to silence the compiler; it disables checking transitively and hides real bugs.

### 2. Model unions, then narrow
- ✅ **APPLY:** Use discriminated unions (a shared literal `kind` field) and switch on it so the compiler exhaustively checks every case.
- ⛔ **AVOID:** Optional booleans that encode state (isLoading/isError/hasData) — they permit impossible combinations.

### 3. Let inference do the work
- ✅ **APPLY:** Annotate function inputs and public API return types; let locals and internals be inferred.
- ⛔ **AVOID:** Over-annotating everything (and re-typing what's already inferred), which drifts out of sync with reality.

### 4. Generics for reusable, type-safe APIs
- ✅ **APPLY:** Use generics with constraints (`<T extends ...>`) and utility types (Partial, Pick, Omit, Record) instead of duplicating shapes.
- ⛔ **AVOID:** Casting with `as` to force a shape — an unchecked assertion that lies to the compiler and crashes at runtime.

### 5. `interface` vs `type`
- ✅ **APPLY:** Use interfaces for extendable object/contract shapes; use type aliases for unions, tuples and mapped/conditional types.
- ⛔ **AVOID:** Enabling loose config — always run with `strict: true` (and noUncheckedIndexedAccess) for real safety.

## Cheat Reference — concepts to remember

- **Prefer `unknown` over `any`** — Accept `unknown` at boundaries and narrow with type guards before use — you keep type safety.
- **Model unions, then narrow** — Use discriminated unions (a shared literal `kind` field) and switch on it so the compiler exhaustively checks every case.
- **Let inference do the work** — Annotate function inputs and public API return types; let locals and internals be inferred.
- **Generics for reusable, type-safe APIs** — Use generics with constraints (`<T extends ...>`) and utility types (Partial, Pick, Omit, Record) instead of duplicating shapes.
- **`interface` vs `type`** — Use interfaces for extendable object/contract shapes; use type aliases for unions, tuples and mapped/conditional types.

## Interview Questions

#### Q1. `any` vs `unknown` vs `never`?
`any` opts out of type checking entirely. `unknown` is the type-safe top type — you must narrow before using it. `never` is the bottom type, representing values that can't occur (exhaustiveness checks, functions that always throw).

#### Q2. Interface vs type alias?
Both describe shapes. Interfaces are open (declaration merging) and ideal for object contracts you may extend; type aliases can also express unions, intersections, tuples, and mapped/conditional types.

#### Q3. What are generics and why use them?
Generics parameterize types so a function or class works across many types while preserving the relationship between inputs and outputs — e.g. `identity<T>(x: T): T` — instead of falling back to `any`.

#### Q4. What is type narrowing?
Refining a broad type to a more specific one within a scope using typeof, instanceof, `in`, equality checks, or custom type guards (`x is Foo`), so the compiler knows the exact type on each branch.

#### Q5. Name useful utility types.
Partial<T>, Required<T>, Readonly<T>, Pick<T,K>, Omit<T,K>, Record<K,V>, ReturnType<F>, Parameters<F>, Awaited<T> — they derive new types from existing ones without duplication.

---

_Curated by Ali Arsalan · https://aliarsalan177.github.io · Generated from the Engineering Guides._


<hr>

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


<hr>

# Agent Guide — Angular

> A batteries-included framework: components, DI, RxJS, and (increasingly) signals. Lean on the structure, mind change detection.
> Category: Frontend · Source: https://aliarsalan177.github.io/guides/angular/

## Purpose

A field guide for **AI coding agents** (and engineers): the concepts to apply and the
mistakes to avoid when building with Angular. Load this into your agent's context so it
keeps these concepts in mind and does **not** repeat these mistakes.

## Rules — Apply / Avoid

### 1. Dependency Injection is the backbone
- ✅ **APPLY:** Put shared logic in @Injectable services and inject via the constructor; scope providers deliberately (root vs component).
- ⛔ **AVOID:** Instantiating services with `new` or duplicating provider registrations — you lose singletons and DI benefits.

### 2. Unsubscribe / use the async pipe
- ✅ **APPLY:** Prefer the `| async` pipe so Angular manages subscription lifecycle, or takeUntil/takeUntilDestroyed for manual ones.
- ⛔ **AVOID:** Subscribing in components without tearing down — long-lived subscriptions leak memory and fire after destroy.

### 3. Change detection has a cost
- ✅ **APPLY:** Use OnPush change detection with immutable inputs (or signals) so Angular checks only what actually changed.
- ⛔ **AVOID:** Doing heavy work or creating new object references in templates/getters — it runs on every CD cycle.

### 4. Reactive forms for anything non-trivial
- ✅ **APPLY:** Use reactive (typed) forms for validation, dynamic controls and testability.
- ⛔ **AVOID:** Forgetting to import FormsModule/ReactiveFormsModule, or mixing template- and reactive-driven on one control.

## Cheat Reference — concepts to remember

- **Dependency Injection is the backbone** — Put shared logic in @Injectable services and inject via the constructor; scope providers deliberately (root vs component).
- **Unsubscribe / use the async pipe** — Prefer the `| async` pipe so Angular manages subscription lifecycle, or takeUntil/takeUntilDestroyed for manual ones.
- **Change detection has a cost** — Use OnPush change detection with immutable inputs (or signals) so Angular checks only what actually changed.
- **Reactive forms for anything non-trivial** — Use reactive (typed) forms for validation, dynamic controls and testability.

## Interview Questions

#### Q1. What is dependency injection in Angular?
A system where Angular creates and supplies service instances to components/services that declare them in their constructor. Providers control scope and lifetime (root-level singletons vs per-component instances).

#### Q2. Structural vs attribute directives?
Structural directives (*ngIf, *ngFor, *ngSwitch) add/remove DOM. Attribute directives (ngClass, ngStyle, ngModel) change the appearance or behavior of an existing element.

#### Q3. What is OnPush change detection?
A strategy that only re-checks a component when its @Input references change, an event fires from it, or an observable it uses (via async pipe) emits — reducing needless checks. It pairs with immutable data or signals.

#### Q4. Template-driven vs reactive forms?
Template-driven forms live in the template with ngModel (simple cases); reactive forms define the model in TypeScript with FormGroup/FormControl, giving explicit, typed, testable control and dynamic validation.

---

_Curated by Ali Arsalan · https://aliarsalan177.github.io · Generated from the Engineering Guides._


<hr>

# Agent Guide — React Native

> React for native mobile. Same mental model, different primitives — think in native components, lists and platform differences.
> Category: Frontend · Source: https://aliarsalan177.github.io/guides/react-native/

## Purpose

A field guide for **AI coding agents** (and engineers): the concepts to apply and the
mistakes to avoid when building with React Native. Load this into your agent's context so it
keeps these concepts in mind and does **not** repeat these mistakes.

## Rules — Apply / Avoid

### 1. Use native primitives, not web tags
- ✅ **APPLY:** Compose UIs from View, Text, Image, Pressable; all text must live inside <Text>.
- ⛔ **AVOID:** Reaching for div/span/CSS files — there's no DOM; styles are JS objects via StyleSheet.

### 2. Virtualize long lists
- ✅ **APPLY:** Use FlatList/SectionList with keyExtractor and getItemLayout for large data; they render only what's visible.
- ⛔ **AVOID:** Mapping thousands of rows inside a ScrollView — it mounts everything at once and janks or crashes.

### 3. Design for both platforms
- ✅ **APPLY:** Branch with Platform.select / Platform.OS and test on iOS and Android; respect safe areas.
- ⛔ **AVOID:** Assuming iOS behavior on Android (or vice versa) for gestures, shadows, and status bars.

### 4. Keep the JS thread free
- ✅ **APPLY:** Memoize rows, use native driver for animations (Reanimated), and offload heavy work.
- ⛔ **AVOID:** Blocking the JS thread with sync work during scroll/animation — it drops frames.

## Cheat Reference — concepts to remember

- **Use native primitives, not web tags** — Compose UIs from View, Text, Image, Pressable; all text must live inside <Text>.
- **Virtualize long lists** — Use FlatList/SectionList with keyExtractor and getItemLayout for large data; they render only what's visible.
- **Design for both platforms** — Branch with Platform.select / Platform.OS and test on iOS and Android; respect safe areas.
- **Keep the JS thread free** — Memoize rows, use native driver for animations (Reanimated), and offload heavy work.

## Interview Questions

#### Q1. How does React Native differ from React for web?
Same component model and hooks, but the primitives are native (View/Text/Image instead of div/span), styling is JS objects via StyleSheet (a Flexbox subset), and it bridges to native platform APIs rather than the DOM.

#### Q2. Why use FlatList over ScrollView?
FlatList virtualizes — it only renders items near the viewport and recycles them — so it scales to large datasets. ScrollView renders all children up front, which is fine only for small, bounded content.

#### Q3. How do you write platform-specific code?
Use the Platform module (Platform.OS / Platform.select) for small branches, or file extensions (.ios.tsx / .android.tsx) for whole-component variants.

---

_Curated by Ali Arsalan · https://aliarsalan177.github.io · Generated from the Engineering Guides._


<hr>

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

## Interview Questions

#### Q1. What is visual hierarchy and how do you create it?
It's the order in which the eye perceives elements. You create it with contrast in size, weight, color, position and whitespace so the most important content is seen first.

#### Q2. How do you ensure a design is accessible?
Meet WCAG color-contrast ratios, provide keyboard focus states, use semantic labels/roles for screen readers, size touch/click targets adequately, and never rely on color alone to convey meaning.

#### Q3. Why use a spacing/grid system?
A base unit (e.g. 8px) and a column grid create consistent rhythm and alignment, make layouts responsive and predictable, and speed up decisions by removing arbitrary values.

---

_Curated by Ali Arsalan · https://aliarsalan177.github.io · Generated from the Engineering Guides._


<hr>

# Agent Guide — AI & LLM Agents

> Prompting, RAG, tools and the agent loop. Ground your models, constrain your outputs, and budget your steps.
> Category: AI & Data · Source: https://aliarsalan177.github.io/guides/ai-agents/

## Purpose

A field guide for **AI coding agents** (and engineers): the concepts to apply and the
mistakes to avoid when building with AI & LLM Agents. Load this into your agent's context so it
keeps these concepts in mind and does **not** repeat these mistakes.

## Rules — Apply / Avoid

### 1. One clear objective + a strict output schema
- ✅ **APPLY:** State a single goal with success criteria and mandate an exact JSON schema; validate the parse and add stop sequences.
- ⛔ **AVOID:** Vague, multi-part asks with no format spec — you get rambling, unparseable, inconsistent output.

### 2. Ground to fight hallucination
- ✅ **APPLY:** Use RAG/tools for facts, require citations, lower temperature, and add an explicit 'if unsure, say I don't know' rule.
- ⛔ **AVOID:** Trusting the model's memory for evolving or proprietary facts — it will confidently invent them.

### 3. Type your tools
- ✅ **APPLY:** Give tools typed argument schemas and validate before executing; the model returns name + JSON args.
- ⛔ **AVOID:** Letting the model free-form tool calls — it hallucinates function names and arguments that then crash.

### 4. Budget the agent loop
- ✅ **APPLY:** Perceive → plan (≤1 line) → act → observe → update, with hard step/cost/time limits and human escalation on low confidence.
- ⛔ **AVOID:** Unbounded agent loops with no budget — they spin, burn tokens, and drift off task.

### 5. Defend against prompt injection
- ✅ **APPLY:** Wrap untrusted content in delimiters marked 'do not execute', and restate the governing rules in the system message.
- ⛔ **AVOID:** Concatenating user/web content straight into the prompt — it can override your instructions.

## Cheat Reference — concepts to remember

- **One clear objective + a strict output schema** — State a single goal with success criteria and mandate an exact JSON schema; validate the parse and add stop sequences.
- **Ground to fight hallucination** — Use RAG/tools for facts, require citations, lower temperature, and add an explicit 'if unsure, say I don't know' rule.
- **Type your tools** — Give tools typed argument schemas and validate before executing; the model returns name + JSON args.
- **Budget the agent loop** — Perceive → plan (≤1 line) → act → observe → update, with hard step/cost/time limits and human escalation on low confidence.
- **Defend against prompt injection** — Wrap untrusted content in delimiters marked 'do not execute', and restate the governing rules in the system message.

## Interview Questions

#### Q1. What is RAG and when should you use it?
Retrieval-Augmented Generation retrieves relevant chunks from an external store (via embeddings) and grounds the model's answer in them with citations. Use it for fresh, proprietary, or verifiable facts instead of relying on training memory.

#### Q2. RAG vs fine-tuning?
RAG injects knowledge at inference time (great for changing facts and sources); fine-tuning bakes in tone, format, and narrow-task behavior. A common pattern is RAG for facts + a light fine-tune (e.g. LoRA) for style/schema.

#### Q3. How do you reduce hallucinations?
Ground with retrieval/tools, require citations, lower temperature, constrain output to a schema, and add an explicit 'say I don't know if unsure' instruction plus validation of the result.

#### Q4. What does the agent loop look like?
Perception → plan → act (tool call) → observe → update state → repeat, with typed tool schemas, short/long-term memory, and enforced step/cost budgets plus escalation when uncertainty is high.

#### Q5. What is temperature?
A sampling parameter: low (0–0.3) makes output more deterministic and factual; higher (0.7–1.0) more creative and varied. Pair with top-p/top-k and penalties to shape the distribution.

---

_Curated by Ali Arsalan · https://aliarsalan177.github.io · Generated from the Engineering Guides._


<hr>

# Agent Guide — Node.js

> Event-driven, non-blocking JavaScript on the server. Keep the loop free, handle every rejection, and stream big data.
> Category: Backend & Infra · Source: https://aliarsalan177.github.io/guides/nodejs/

## Purpose

A field guide for **AI coding agents** (and engineers): the concepts to apply and the
mistakes to avoid when building with Node.js. Load this into your agent's context so it
keeps these concepts in mind and does **not** repeat these mistakes.

## Rules — Apply / Avoid

### 1. Never block the event loop
- ✅ **APPLY:** Use async APIs; offload CPU-heavy work to worker threads or a queue.
- ⛔ **AVOID:** Sync calls (fs.readFileSync, big JSON.parse, crypto in a request path) — they stall every other request.

### 2. Handle every async error
- ✅ **APPLY:** await inside try/catch, attach .catch to promises, and listen for 'error' on streams/emitters.
- ⛔ **AVOID:** Unhandled promise rejections — they can crash the process and leave requests hanging.

### 3. Stream large payloads
- ✅ **APPLY:** Pipe files/HTTP bodies through streams so memory stays flat regardless of size.
- ⛔ **AVOID:** Buffering a whole file/response into memory — it spikes RAM and falls over under load.

### 4. Config via environment, secrets out of code
- ✅ **APPLY:** Read config from process.env; commit package-lock.json for reproducible installs.
- ⛔ **AVOID:** Hard-coding secrets or shipping them in package.json / the repo.

## Cheat Reference — concepts to remember

- **Never block the event loop** — Use async APIs; offload CPU-heavy work to worker threads or a queue.
- **Handle every async error** — await inside try/catch, attach .catch to promises, and listen for 'error' on streams/emitters.
- **Stream large payloads** — Pipe files/HTTP bodies through streams so memory stays flat regardless of size.
- **Config via environment, secrets out of code** — Read config from process.env; commit package-lock.json for reproducible installs.

## Interview Questions

#### Q1. How does Node handle concurrency if it's single-threaded?
JS runs on one thread, but I/O is delegated to libuv's thread pool and the OS; the event loop schedules callbacks when operations complete. This non-blocking model handles many connections without a thread per request.

#### Q2. What happens if you block the event loop?
All other callbacks — including incoming requests — wait. A single synchronous CPU-bound or sync-I/O call freezes the whole server, so such work must be async, chunked, or moved to worker threads.

#### Q3. Why prefer streams?
Streams process data in chunks, keeping memory usage constant and starting output sooner, which is essential for large files, uploads, and proxying without exhausting RAM.

#### Q4. CommonJS vs ES Modules?
CommonJS uses require/module.exports and loads synchronously (Node's classic system); ESM uses import/export, is statically analyzable and async-friendly. Pick one per project and stay consistent.

---

_Curated by Ali Arsalan · https://aliarsalan177.github.io · Generated from the Engineering Guides._


<hr>

# Agent Guide — Python

> Readable, batteries-included, and full of sharp edges around mutability and scope. Write idiomatic, explicit Python.
> Category: Languages · Source: https://aliarsalan177.github.io/guides/python/

## Purpose

A field guide for **AI coding agents** (and engineers): the concepts to apply and the
mistakes to avoid when building with Python. Load this into your agent's context so it
keeps these concepts in mind and does **not** repeat these mistakes.

## Rules — Apply / Avoid

### 1. Never use mutable default arguments
- ✅ **APPLY:** Default to None and create the list/dict inside the function body.
- ⛔ **AVOID:** def f(x, acc=[]) — the default is created once and shared across calls, accumulating state between them.

### 2. Know mutability and copies
- ✅ **APPLY:** Use copy.deepcopy for nested structures; remember list slicing/`.copy()` are shallow.
- ⛔ **AVOID:** Assuming a shallow copy isolates you — nested objects still point at the originals.

### 3. Prefer comprehensions and built-ins
- ✅ **APPLY:** Use list/dict/set comprehensions and generators (yield) for clear, memory-efficient iteration.
- ⛔ **AVOID:** Manual index loops that rebuild what map/filter/enumerate/zip already do idiomatically.

### 4. Use context managers for resources
- ✅ **APPLY:** Open files/connections with `with` so they close even on exceptions; catch specific exceptions.
- ⛔ **AVOID:** Bare `except:` that swallows everything (including KeyboardInterrupt) and hides real failures.

## Cheat Reference — concepts to remember

- **Never use mutable default arguments** — Default to None and create the list/dict inside the function body.
- **Know mutability and copies** — Use copy.deepcopy for nested structures; remember list slicing/`.copy()` are shallow.
- **Prefer comprehensions and built-ins** — Use list/dict/set comprehensions and generators (yield) for clear, memory-efficient iteration.
- **Use context managers for resources** — Open files/connections with `with` so they close even on exceptions; catch specific exceptions.

## Interview Questions

#### Q1. Why are mutable default arguments dangerous?
Defaults are evaluated once at definition time, so a default list/dict is shared across all calls and accumulates state. Use None as the default and create the object inside the function.

#### Q2. Explain the LEGB scope rule.
Name lookup goes Local → Enclosing → Global → Built-in. Assigning to a name makes it local unless declared global or nonlocal, which trips up many beginners.

#### Q3. What are generators and why use them?
Functions that yield values lazily, one at a time, implementing the iterator protocol. They're memory-efficient for large or infinite sequences since they don't materialize the whole collection.

#### Q4. What is a decorator?
A callable that takes a function and returns a wrapped function with added behavior (logging, caching, auth), applied with @syntax — composition of functions without editing the original.

---

_Curated by Ali Arsalan · https://aliarsalan177.github.io · Generated from the Engineering Guides._


<hr>

# Agent Guide — Rust

> Memory safety without a garbage collector, enforced at compile time. Learn to think in ownership, borrowing and lifetimes.
> Category: Languages · Source: https://aliarsalan177.github.io/guides/rust/

## Purpose

A field guide for **AI coding agents** (and engineers): the concepts to apply and the
mistakes to avoid when building with Rust. Load this into your agent's context so it
keeps these concepts in mind and does **not** repeat these mistakes.

## Rules — Apply / Avoid

### 1. Ownership and moves
- ✅ **APPLY:** Let one owner hold each value; pass by reference to lend it, or clone when you truly need a copy.
- ⛔ **AVOID:** Using a value after it's been moved — the compiler rejects it; restructure or borrow instead.

### 2. Borrowing rules
- ✅ **APPLY:** Hold many immutable (&) borrows or exactly one mutable (&mut) borrow at a time.
- ⛔ **AVOID:** Mixing a mutable and immutable borrow of the same data simultaneously — the borrow checker forbids it.

### 3. Model absence and failure in the type system
- ✅ **APPLY:** Use Option<T> for maybe-present values and Result<T,E> for fallible ops; propagate with `?`.
- ⛔ **AVOID:** Reaching for .unwrap()/.expect() in real code paths — it panics instead of handling the error.

### 4. Traits over inheritance
- ✅ **APPLY:** Define shared behavior with traits and generics with trait bounds.
- ⛔ **AVOID:** Fighting the borrow checker with excessive clone()/Rc everywhere — usually the design needs rethinking.

## Cheat Reference — concepts to remember

- **Ownership and moves** — Let one owner hold each value; pass by reference to lend it, or clone when you truly need a copy.
- **Borrowing rules** — Hold many immutable (&) borrows or exactly one mutable (&mut) borrow at a time.
- **Model absence and failure in the type system** — Use Option<T> for maybe-present values and Result<T,E> for fallible ops; propagate with `?`.
- **Traits over inheritance** — Define shared behavior with traits and generics with trait bounds.

## Interview Questions

#### Q1. What is ownership in Rust?
Each value has a single owner; when the owner goes out of scope the value is dropped. Assigning or passing a non-Copy value moves ownership, invalidating the original binding — this gives memory safety without a GC.

#### Q2. What are the borrowing rules?
At any time you can have either any number of immutable references (&T) or exactly one mutable reference (&mut T), but not both. This prevents data races and aliasing bugs at compile time.

#### Q3. Option and Result vs null/exceptions?
Option<T> encodes presence/absence and Result<T,E> encodes success/failure in the type system, forcing you to handle both cases. There's no null and no exceptions; the `?` operator ergonomically propagates errors.

#### Q4. What are lifetimes?
Annotations that tell the compiler how long references must remain valid so it can guarantee no reference outlives the data it points to, preventing dangling references.

---

_Curated by Ali Arsalan · https://aliarsalan177.github.io · Generated from the Engineering Guides._


<hr>

# Agent Guide — Solidity & Smart Contracts

> Code that controls money on an immutable ledger. Security and gas are first-class concerns, not afterthoughts.
> Category: Languages · Source: https://aliarsalan177.github.io/guides/solidity/

## Purpose

A field guide for **AI coding agents** (and engineers): the concepts to apply and the
mistakes to avoid when building with Solidity & Smart Contracts. Load this into your agent's context so it
keeps these concepts in mind and does **not** repeat these mistakes.

## Rules — Apply / Avoid

### 1. Checks-Effects-Interactions
- ✅ **APPLY:** Validate inputs, update state, then make external calls last — and consider a reentrancy guard.
- ⛔ **AVOID:** Calling an external contract before updating your state — the classic reentrancy vulnerability.

### 2. Authenticate with msg.sender
- ✅ **APPLY:** Gate privileged functions on msg.sender (the immediate caller) via modifiers.
- ⛔ **AVOID:** Using tx.origin for auth — it's the original EOA and is phishable through intermediary contracts.

### 3. Mind storage, gas and visibility
- ✅ **APPLY:** Minimize storage writes, use events for logs, mark visibility explicitly, and use custom errors over revert strings.
- ⛔ **AVOID:** Unbounded loops over storage or needless writes — they blow the gas limit and can brick a function.

### 4. Fail loudly and early
- ✅ **APPLY:** Use require/custom errors to validate preconditions and revert on invalid state.
- ⛔ **AVOID:** Assuming external calls succeed — check return values and handle the failure path.

## Cheat Reference — concepts to remember

- **Checks-Effects-Interactions** — Validate inputs, update state, then make external calls last — and consider a reentrancy guard.
- **Authenticate with msg.sender** — Gate privileged functions on msg.sender (the immediate caller) via modifiers.
- **Mind storage, gas and visibility** — Minimize storage writes, use events for logs, mark visibility explicitly, and use custom errors over revert strings.
- **Fail loudly and early** — Use require/custom errors to validate preconditions and revert on invalid state.

## Interview Questions

#### Q1. What is a reentrancy attack and how do you prevent it?
An external call lets the callee re-enter your function before its state updates, draining funds. Prevent it with the checks-effects-interactions order (update state before external calls) and/or a reentrancy guard.

#### Q2. Why prefer msg.sender over tx.origin?
msg.sender is the immediate caller; tx.origin is the original externally-owned account of the whole transaction. Using tx.origin for authorization lets a malicious intermediary contract act on a user's behalf (phishing).

#### Q3. storage vs memory?
storage is persistent on-chain state (expensive to write); memory is temporary within a function call. Choosing correctly affects both correctness and gas cost.

#### Q4. How do you save gas?
Minimize storage writes, emit events instead of storing logs, use custom errors, pack variables, avoid unbounded loops, and use immutable/constant where possible.

---

_Curated by Ali Arsalan · https://aliarsalan177.github.io · Generated from the Engineering Guides._


<hr>

# Agent Guide — Data Structures & Algorithms

> Pick the right structure, know the complexity, and recognize the pattern. The foundation of performant code and interviews.
> Category: CS & Security · Source: https://aliarsalan177.github.io/guides/dsa/

## Purpose

A field guide for **AI coding agents** (and engineers): the concepts to apply and the
mistakes to avoid when building with Data Structures & Algorithms. Load this into your agent's context so it
keeps these concepts in mind and does **not** repeat these mistakes.

## Rules — Apply / Avoid

### 1. Reason in Big-O
- ✅ **APPLY:** Know the time/space cost of your operations and choose structures accordingly (hash map for O(1) lookup, etc.).
- ⛔ **AVOID:** Nesting loops into O(n²) when a hash set/map turns it into O(n) — the classic scaling trap.

### 2. Match structure to access pattern
- ✅ **APPLY:** Arrays for indexed access, hash tables for keyed lookup, heaps for priorities, trees/graphs for hierarchy and connections.
- ⛔ **AVOID:** Scanning a list for membership repeatedly when a set gives O(1) — quadratic behavior in disguise.

### 3. Learn the core patterns
- ✅ **APPLY:** Two pointers and sliding window for arrays/strings, BFS/DFS for graphs/trees, DP for overlapping subproblems.
- ⛔ **AVOID:** Recomputing overlapping subproblems — memoize or tabulate to collapse exponential work to polynomial.

### 4. Watch recursion depth
- ✅ **APPLY:** Bound recursion or convert to iteration with an explicit stack for deep inputs.
- ⛔ **AVOID:** Unbounded recursion that blows the call stack ('maximum call stack size exceeded').

## Cheat Reference — concepts to remember

- **Reason in Big-O** — Know the time/space cost of your operations and choose structures accordingly (hash map for O(1) lookup, etc.).
- **Match structure to access pattern** — Arrays for indexed access, hash tables for keyed lookup, heaps for priorities, trees/graphs for hierarchy and connections.
- **Learn the core patterns** — Two pointers and sliding window for arrays/strings, BFS/DFS for graphs/trees, DP for overlapping subproblems.
- **Watch recursion depth** — Bound recursion or convert to iteration with an explicit stack for deep inputs.

## Interview Questions

#### Q1. Why are hash tables O(1) average but O(n) worst case?
A good hash spreads keys into buckets for constant-time access, but collisions (or a bad hash / resizing) can degrade to linear when many keys land in one bucket.

#### Q2. When would you use binary search?
On sorted data, to find an element (or boundary) in O(log n) by halving the search space each step — far better than an O(n) linear scan.

#### Q3. BFS vs DFS?
BFS explores level by level (queue) and finds shortest paths in unweighted graphs; DFS goes deep first (stack/recursion) and suits path-finding, cycle detection, and topological sorts. Both are O(V+E).

#### Q4. What is dynamic programming?
Solving a problem by combining solutions to overlapping subproblems, caching results (memoization) or building them up (tabulation) to avoid exponential recomputation.

---

_Curated by Ali Arsalan · https://aliarsalan177.github.io · Generated from the Engineering Guides._


<hr>

# Agent Guide — Cybersecurity

> Protect confidentiality, integrity and availability. Assume breach, least privilege, and defense in depth.
> Category: CS & Security · Source: https://aliarsalan177.github.io/guides/cybersecurity/

## Purpose

A field guide for **AI coding agents** (and engineers): the concepts to apply and the
mistakes to avoid when building with Cybersecurity. Load this into your agent's context so it
keeps these concepts in mind and does **not** repeat these mistakes.

## Rules — Apply / Avoid

### 1. Never trust input
- ✅ **APPLY:** Validate and sanitize all input; use parameterized queries and output encoding.
- ⛔ **AVOID:** Concatenating user input into SQL or HTML — the root of SQL injection and XSS.

### 2. Least privilege & defense in depth
- ✅ **APPLY:** Grant the minimum access needed and layer controls (network, app, data) so one failure isn't fatal.
- ⛔ **AVOID:** Broad admin credentials and a single perimeter — one breach then owns everything.

### 3. Handle secrets and crypto correctly
- ✅ **APPLY:** Hash passwords with a slow salted KDF (bcrypt/argon2), encrypt in transit and at rest, use vetted libraries.
- ⛔ **AVOID:** Rolling your own crypto, storing plaintext/MD5 passwords, or committing secrets to the repo.

### 4. Authenticate then authorize
- ✅ **APPLY:** Verify identity (MFA) and separately check permissions on every request, server-side.
- ⛔ **AVOID:** Trusting client-side checks or a hidden UI for security — attackers call the API directly.

## Cheat Reference — concepts to remember

- **Never trust input** — Validate and sanitize all input; use parameterized queries and output encoding.
- **Least privilege & defense in depth** — Grant the minimum access needed and layer controls (network, app, data) so one failure isn't fatal.
- **Handle secrets and crypto correctly** — Hash passwords with a slow salted KDF (bcrypt/argon2), encrypt in transit and at rest, use vetted libraries.
- **Authenticate then authorize** — Verify identity (MFA) and separately check permissions on every request, server-side.

## Interview Questions

#### Q1. What is the CIA triad?
Confidentiality (only authorized access), Integrity (data isn't tampered with), and Availability (systems are up when needed) — the three goals that frame most security decisions.

#### Q2. How do you prevent SQL injection and XSS?
SQL injection: use parameterized queries/ORMs and validate input. XSS: encode/escape output for its context and use a Content-Security-Policy. Both stem from treating untrusted input as trusted code.

#### Q3. Symmetric vs asymmetric encryption?
Symmetric uses one shared secret key (fast, for bulk data); asymmetric uses a public/private key pair (for key exchange and signatures). TLS combines both: asymmetric to agree on a symmetric session key.

#### Q4. Why hash passwords with bcrypt/argon2 instead of SHA-256?
Password hashes should be deliberately slow and salted to resist brute-force and rainbow tables. Fast general-purpose hashes (SHA-256, MD5) are trivially crackable at scale.

---

_Curated by Ali Arsalan · https://aliarsalan177.github.io · Generated from the Engineering Guides._


<hr>

# Agent Guide — Nmap (Network Scanning)

> The standard tool for host discovery and port scanning. Powerful for defense and recon — only ever with authorization.
> Category: CS & Security · Source: https://aliarsalan177.github.io/guides/nmap/

## Purpose

A field guide for **AI coding agents** (and engineers): the concepts to apply and the
mistakes to avoid when building with Nmap (Network Scanning). Load this into your agent's context so it
keeps these concepts in mind and does **not** repeat these mistakes.

## Rules — Apply / Avoid

### 1. Authorization first
- ✅ **APPLY:** Only scan networks you own or have explicit written permission to test.
- ⛔ **AVOID:** Scanning third-party or public hosts without consent — it's noisy, detectable, and often illegal.

### 2. Pick the right scan for the job
- ✅ **APPLY:** Use -sS (stealth SYN) for TCP, -sU for UDP services, and scope ports with -p / -F / -p-.
- ⛔ **AVOID:** Blindly running -A -T5 on production — it's loud, heavy, and can disrupt fragile services.

### 3. Add detection, then verify
- ✅ **APPLY:** Layer -sV (versions) and -O (OS) and targeted NSE scripts, then confirm findings manually.
- ⛔ **AVOID:** Treating raw scan output as ground truth — versions and OS guesses can be wrong.

## Cheat Reference — concepts to remember

- **Authorization first** — Only scan networks you own or have explicit written permission to test.
- **Pick the right scan for the job** — Use -sS (stealth SYN) for TCP, -sU for UDP services, and scope ports with -p / -F / -p-.
- **Add detection, then verify** — Layer -sV (versions) and -O (OS) and targeted NSE scripts, then confirm findings manually.

## Interview Questions

#### Q1. What's the difference between a SYN scan and a TCP connect scan?
A SYN scan (-sS) sends SYN and never completes the handshake — faster and stealthier. A connect scan (-sT) completes the full three-way handshake, so it's more detectable and logged.

#### Q2. How do you detect services and OS with Nmap?
-sV probes open ports to fingerprint service versions; -O analyzes TCP/IP stack behavior to guess the OS (needs at least one open and one closed port); -A bundles both plus scripts and traceroute.

#### Q3. What do timing templates (-T0..-T5) do?
They trade speed for stealth/accuracy: T0–T1 are slow and evasive, T3 is the default, T4–T5 are fast and aggressive but noisier and more error-prone.

---

_Curated by Ali Arsalan · https://aliarsalan177.github.io · Generated from the Engineering Guides._


<hr>

# Agent Guide — Linux Command Line

> The operator's toolkit: navigate, permission, pipe, and manage processes with confidence — carefully.
> Category: Backend & Infra · Source: https://aliarsalan177.github.io/guides/linux/

## Purpose

A field guide for **AI coding agents** (and engineers): the concepts to apply and the
mistakes to avoid when building with Linux Command Line. Load this into your agent's context so it
keeps these concepts in mind and does **not** repeat these mistakes.

## Rules — Apply / Avoid

### 1. Compose small tools with pipes
- ✅ **APPLY:** Chain commands with | and redirect with > / >> / 2>&1 to build powerful one-liners (grep | sort | uniq).
- ⛔ **AVOID:** Parsing structured data with fragile ad-hoc text hacks when a proper tool (jq, awk) fits better.

### 2. Respect destructive commands
- ✅ **APPLY:** Double-check paths; prefer rm -i, and test globs with ls before rm -rf.
- ⛔ **AVOID:** rm -rf on an unverified or variable path (rm -rf $DIR/ with empty $DIR) — it's irreversible.

### 3. Least-privilege permissions
- ✅ **APPLY:** Set the minimum chmod/chown needed; use sudo deliberately for single commands.
- ⛔ **AVOID:** chmod 777 to 'make it work' — it opens files to everyone and is a security hole.

### 4. Manage processes and services
- ✅ **APPLY:** Inspect with ps aux/top, signal with kill, and run services via systemctl; schedule with cron.
- ⛔ **AVOID:** kill -9 as a first resort — it skips cleanup; try a graceful signal first.

## Cheat Reference — concepts to remember

- **Compose small tools with pipes** — Chain commands with | and redirect with > / >> / 2>&1 to build powerful one-liners (grep | sort | uniq).
- **Respect destructive commands** — Double-check paths; prefer rm -i, and test globs with ls before rm -rf.
- **Least-privilege permissions** — Set the minimum chmod/chown needed; use sudo deliberately for single commands.
- **Manage processes and services** — Inspect with ps aux/top, signal with kill, and run services via systemctl; schedule with cron.

## Interview Questions

#### Q1. What do chmod 755 and 644 mean?
Three octal digits for owner/group/others where r=4, w=2, x=1. 755 = owner rwx, group/others r-x (typical for executables/dirs); 644 = owner rw, others r-- (typical for files).

#### Q2. Explain pipes and redirection.
`|` sends one command's stdout to the next command's stdin; `>` writes stdout to a file (overwrite), `>>` appends, and `2>&1` merges stderr into stdout so both are captured together.

#### Q3. How do you find and kill a process?
Locate it with ps aux | grep name or top, note the PID, then kill <pid> (graceful SIGTERM). Use kill -9 (SIGKILL) only if it won't terminate, since it skips cleanup.

---

_Curated by Ali Arsalan · https://aliarsalan177.github.io · Generated from the Engineering Guides._


<hr>

# Agent Guide — Business Analytics

> Turn data into decisions. Know your analytics type, clean your data, and never confuse correlation with causation.
> Category: AI & Data · Source: https://aliarsalan177.github.io/guides/business-analytics/

## Purpose

A field guide for **AI coding agents** (and engineers): the concepts to apply and the
mistakes to avoid when building with Business Analytics. Load this into your agent's context so it
keeps these concepts in mind and does **not** repeat these mistakes.

## Rules — Apply / Avoid

### 1. Match the analytics type to the question
- ✅ **APPLY:** Descriptive (what happened), diagnostic (why), predictive (what will), prescriptive (what to do) — pick before you model.
- ⛔ **AVOID:** Jumping to prediction on dirty or unrepresentative data — garbage in, confident garbage out.

### 2. Correlation is not causation
- ✅ **APPLY:** Establish causality with experiments (A/B tests) or quasi-experimental methods (diff-in-diff, matching).
- ⛔ **AVOID:** Acting on a correlation as if it were causal — confounders will burn you.

### 3. Choose robust summary statistics
- ✅ **APPLY:** Use the median for skewed data, report dispersion (std dev), and check distribution before applying tests.
- ⛔ **AVOID:** Reporting only the mean on skewed/outlier-heavy data — it misleads.

### 4. Measure what matters
- ✅ **APPLY:** Define a few actionable KPIs tied to outcomes; segment (RFM, cohorts, funnels) to find the story.
- ⛔ **AVOID:** Vanity metrics and cluttered dashboards that look busy but drive no decision.

## Cheat Reference — concepts to remember

- **Match the analytics type to the question** — Descriptive (what happened), diagnostic (why), predictive (what will), prescriptive (what to do) — pick before you model.
- **Correlation is not causation** — Establish causality with experiments (A/B tests) or quasi-experimental methods (diff-in-diff, matching).
- **Choose robust summary statistics** — Use the median for skewed data, report dispersion (std dev), and check distribution before applying tests.
- **Measure what matters** — Define a few actionable KPIs tied to outcomes; segment (RFM, cohorts, funnels) to find the story.

## Interview Questions

#### Q1. What are the four types of analytics?
Descriptive (what happened), diagnostic (why it happened), predictive (what's likely next), and prescriptive (what action to take). Each answers a different business question and needs different methods.

#### Q2. Why is correlation not causation?
Two variables can move together due to a confounder or coincidence without one causing the other. Establishing causation needs controlled experiments (A/B tests) or quasi-experimental designs, not correlation alone.

#### Q3. Mean vs median — when to use which?
The mean is the balance point for roughly symmetric data; the median is robust to outliers and skew, so it better represents 'typical' values in skewed distributions like income or response times.

#### Q4. What is an A/B test?
A randomized experiment splitting users into control and variant to isolate the causal effect of a change, measured on a predefined metric with statistical significance before rolling out.

---

_Curated by Ali Arsalan · https://aliarsalan177.github.io · Generated from the Engineering Guides._


<hr>

# Agent Guide — Go (Golang)

> Built for scale and simplicity. Lightweight concurrency, explicit errors, and composition over inheritance.
> Category: Languages · Source: https://aliarsalan177.github.io/guides/golang/

## Purpose

A field guide for **AI coding agents** (and engineers): the concepts to apply and the
mistakes to avoid when building with Go (Golang). Load this into your agent's context so it
keeps these concepts in mind and does **not** repeat these mistakes.

## Rules — Apply / Avoid

### 1. Concurrency via goroutines + channels
- ✅ **APPLY:** Launch work with `go`, coordinate with channels, and wait with sync.WaitGroup; share by communicating.
- ⛔ **AVOID:** Firing goroutines the main function never waits for — they're killed on exit, silently dropping work.

### 2. Handle every error explicitly
- ✅ **APPLY:** Check the returned err before using the result; wrap with context (fmt.Errorf("...: %w", err)).
- ⛔ **AVOID:** Ignoring the error return (`val, _ :=`) — Go has no exceptions, so a dropped error is a hidden failure.

### 3. Guard shared state
- ✅ **APPLY:** Protect shared data with a sync.Mutex (defer Unlock) or hand ownership through a channel.
- ⛔ **AVOID:** Concurrent map writes / unsynchronized access — it's a data race and can crash the program.

### 4. Interfaces are satisfied implicitly
- ✅ **APPLY:** Define small interfaces and let types satisfy them by implementing methods; compose with embedding.
- ⛔ **AVOID:** Mixing pointer and value receivers on one type, which quietly changes what satisfies an interface.

## Cheat Reference — concepts to remember

- **Concurrency via goroutines + channels** — Launch work with `go`, coordinate with channels, and wait with sync.WaitGroup; share by communicating.
- **Handle every error explicitly** — Check the returned err before using the result; wrap with context (fmt.Errorf("...: %w", err)).
- **Guard shared state** — Protect shared data with a sync.Mutex (defer Unlock) or hand ownership through a channel.
- **Interfaces are satisfied implicitly** — Define small interfaces and let types satisfy them by implementing methods; compose with embedding.

## Interview Questions

#### Q1. What are goroutines and channels?
Goroutines are lightweight, Go-managed green threads started with `go`; channels are typed conduits for communicating between them. The philosophy is 'share memory by communicating' rather than locking shared state.

#### Q2. How does Go handle errors?
Explicitly — functions return (value, error) and callers check the error immediately. There are no exceptions; you propagate errors up the call stack, often wrapping them with context.

#### Q3. Slice vs array in Go?
An array has a fixed compile-time length; a slice is a dynamic view over an underlying array with a length and capacity, supporting append. Slices are what you use in practice.

#### Q4. What does defer do?
Schedules a call to run when the surrounding function returns (LIFO order), ideal for cleanup like closing files or unlocking mutexes even if the function panics.

---

_Curated by Ali Arsalan · https://aliarsalan177.github.io · Generated from the Engineering Guides._


<hr>

# Agent Guide — C#

> A modern, statically-typed .NET language. Value vs reference semantics, LINQ, async/await and nullable reference types.
> Category: Languages · Source: https://aliarsalan177.github.io/guides/csharp/

## Purpose

A field guide for **AI coding agents** (and engineers): the concepts to apply and the
mistakes to avoid when building with C#. Load this into your agent's context so it
keeps these concepts in mind and does **not** repeat these mistakes.

## Rules — Apply / Avoid

### 1. Know value vs reference types
- ✅ **APPLY:** Understand structs (value, copied) vs classes (reference, shared); use records for immutable value-equality data.
- ⛔ **AVOID:** Assuming a struct assignment shares state (it copies) or that class assignment copies (it shares the reference).

### 2. async/await all the way
- ✅ **APPLY:** Return Task/Task<T> and await async calls so threads aren't blocked; avoid mixing sync and async.
- ⛔ **AVOID:** Calling .Result / .Wait() on a Task — it can deadlock and defeats the point of async.

### 3. Enable nullable reference types
- ✅ **APPLY:** Turn on #nullable to make nullability explicit and catch null-deref bugs at compile time.
- ⛔ **AVOID:** Ignoring nullable warnings or overusing the null-forgiving `!` operator to silence them.

### 4. Prefer LINQ, but mind execution
- ✅ **APPLY:** Express queries declaratively with LINQ; remember it's lazily evaluated until materialized (ToList/Count).
- ⛔ **AVOID:** Enumerating the same deferred query repeatedly (re-running the work) or hidden N+1s over a DB provider.

## Cheat Reference — concepts to remember

- **Know value vs reference types** — Understand structs (value, copied) vs classes (reference, shared); use records for immutable value-equality data.
- **async/await all the way** — Return Task/Task<T> and await async calls so threads aren't blocked; avoid mixing sync and async.
- **Enable nullable reference types** — Turn on #nullable to make nullability explicit and catch null-deref bugs at compile time.
- **Prefer LINQ, but mind execution** — Express queries declaratively with LINQ; remember it's lazily evaluated until materialized (ToList/Count).

## Interview Questions

#### Q1. Value types vs reference types?
Value types (int, struct, enum) hold data directly and are copied on assignment; reference types (class, string, array) hold a reference to heap memory, so assignment shares the same object.

#### Q2. What does async/await do?
async marks a method that returns a Task; await asynchronously waits for a Task to finish without blocking the thread, freeing it to do other work — key for scalable I/O.

#### Q3. What are nullable reference types?
A compiler feature (#nullable enable) that distinguishes `string` (non-null) from `string?` (nullable) and warns on possible null dereferences, reducing NullReferenceExceptions.

#### Q4. What is deferred execution in LINQ?
LINQ queries build an expression that isn't executed until enumerated (foreach, ToList, Count). This enables composition but can cause repeated work or surprising results if the source changes.

---

_Curated by Ali Arsalan · https://aliarsalan177.github.io · Generated from the Engineering Guides._


<hr>

# Agent Guide — SQL

> The language of relational data. Master joins, aggregation, indexing and NULL semantics to query correctly and fast.
> Category: Backend & Infra · Source: https://aliarsalan177.github.io/guides/sql/

## Purpose

A field guide for **AI coding agents** (and engineers): the concepts to apply and the
mistakes to avoid when building with SQL. Load this into your agent's context so it
keeps these concepts in mind and does **not** repeat these mistakes.

## Rules — Apply / Avoid

### 1. Always join on a condition
- ✅ **APPLY:** Specify the ON/WHERE relationship for every join; pick the join type (INNER/LEFT) intentionally.
- ⛔ **AVOID:** A missing join condition — it produces a cartesian product that explodes row counts and grinds to a halt.

### 2. NULL is not a value
- ✅ **APPLY:** Test with IS NULL / IS NOT NULL and normalize with COALESCE; know aggregates skip NULLs.
- ⛔ **AVOID:** Comparing with `= NULL` (always false) or forgetting NULLs vanish from counts and equality filters.

### 3. Index for your access patterns
- ✅ **APPLY:** Index columns used in WHERE/JOIN/ORDER BY; read the query plan to confirm they're used.
- ⛔ **AVOID:** Over-indexing (slows writes) or wrapping indexed columns in functions (which disables the index).

### 4. WHERE filters rows, HAVING filters groups
- ✅ **APPLY:** Filter individual rows in WHERE, then filter aggregated groups in HAVING after GROUP BY.
- ⛔ **AVOID:** Putting aggregate conditions (COUNT(*) > 5) in WHERE — aggregates only work in HAVING.

## Cheat Reference — concepts to remember

- **Always join on a condition** — Specify the ON/WHERE relationship for every join; pick the join type (INNER/LEFT) intentionally.
- **NULL is not a value** — Test with IS NULL / IS NOT NULL and normalize with COALESCE; know aggregates skip NULLs.
- **Index for your access patterns** — Index columns used in WHERE/JOIN/ORDER BY; read the query plan to confirm they're used.
- **WHERE filters rows, HAVING filters groups** — Filter individual rows in WHERE, then filter aggregated groups in HAVING after GROUP BY.

## Interview Questions

#### Q1. INNER JOIN vs LEFT JOIN?
INNER JOIN returns only rows matching in both tables. LEFT JOIN returns all rows from the left table plus matches from the right, filling NULLs where there's no match.

#### Q2. WHERE vs HAVING?
WHERE filters rows before grouping and can't use aggregates; HAVING filters after GROUP BY and is where aggregate conditions (e.g., SUM(x) > 100) belong.

#### Q3. How do indexes speed up queries, and what's the trade-off?
An index is a sorted structure (usually a B-tree) enabling O(log n) lookups instead of full scans on indexed columns. The cost is extra storage and slower writes, since every insert/update maintains the index.

#### Q4. Why is NULL handling tricky?
NULL means 'unknown', so comparisons with it yield unknown (not true) — you must use IS NULL. Aggregates ignore NULLs, and they can silently drop rows from equality filters and inner joins.

---

_Curated by Ali Arsalan · https://aliarsalan177.github.io · Generated from the Engineering Guides._


<hr>

# Agent Guide — Excel

> The world's most-used analysis tool. Lookups, aggregation, absolute references and pivot tables — done cleanly.
> Category: AI & Data · Source: https://aliarsalan177.github.io/guides/excel/

## Purpose

A field guide for **AI coding agents** (and engineers): the concepts to apply and the
mistakes to avoid when building with Excel. Load this into your agent's context so it
keeps these concepts in mind and does **not** repeat these mistakes.

## Rules — Apply / Avoid

### 1. Lock references deliberately
- ✅ **APPLY:** Use absolute ($A$1) vs relative (A1) references correctly so formulas copy the way you intend (F4 to cycle).
- ⛔ **AVOID:** Copying a formula and having its references shift unexpectedly, silently corrupting a whole column.

### 2. Prefer XLOOKUP / INDEX-MATCH
- ✅ **APPLY:** Use XLOOKUP or INDEX+MATCH for robust, two-way lookups that survive column insertions.
- ⛔ **AVOID:** VLOOKUP with a hard-coded column index — it breaks the moment someone inserts a column.

### 3. Wrap risky formulas
- ✅ **APPLY:** Guard against errors with IFERROR and combine conditions with IF+AND/OR for clarity.
- ⛔ **AVOID:** Leaving #DIV/0!, #N/A, #REF! errors to propagate through dependent cells and dashboards.

### 4. Summarize with pivot tables
- ✅ **APPLY:** Use pivot tables to aggregate and explore large tables without fragile nested formulas.
- ⛔ **AVOID:** Building huge manual SUMIF/COUNTIF webs where a pivot table would be faster and less error-prone.

## Cheat Reference — concepts to remember

- **Lock references deliberately** — Use absolute ($A$1) vs relative (A1) references correctly so formulas copy the way you intend (F4 to cycle).
- **Prefer XLOOKUP / INDEX-MATCH** — Use XLOOKUP or INDEX+MATCH for robust, two-way lookups that survive column insertions.
- **Wrap risky formulas** — Guard against errors with IFERROR and combine conditions with IF+AND/OR for clarity.
- **Summarize with pivot tables** — Use pivot tables to aggregate and explore large tables without fragile nested formulas.

## Interview Questions

#### Q1. Relative vs absolute cell references?
Relative references (A1) shift when a formula is copied; absolute references ($A$1) stay fixed. Mixed ($A1 / A$1) lock only the row or column. F4 cycles through them.

#### Q2. Why prefer XLOOKUP/INDEX-MATCH over VLOOKUP?
VLOOKUP can only look right and depends on a hard-coded column number that breaks when columns move. XLOOKUP and INDEX-MATCH look in any direction and reference columns by identity, so they're more robust.

#### Q3. What is a pivot table for?
Quickly summarizing, grouping, and cross-tabulating large datasets (sums, counts, averages by category) interactively, without writing complex formulas.

---

_Curated by Ali Arsalan · https://aliarsalan177.github.io · Generated from the Engineering Guides._


<hr>

# Agent Guide — Security+ (Fundamentals)

> The baseline security body of knowledge: risk, identity, cryptography and controls — the CompTIA Security+ core.
> Category: CS & Security · Source: https://aliarsalan177.github.io/guides/security-plus/

## Purpose

A field guide for **AI coding agents** (and engineers): the concepts to apply and the
mistakes to avoid when building with Security+ (Fundamentals). Load this into your agent's context so it
keeps these concepts in mind and does **not** repeat these mistakes.

## Rules — Apply / Avoid

### 1. Think in risk, not just threats
- ✅ **APPLY:** Assess risk = likelihood × impact, then treat it (mitigate, transfer, accept, avoid) and prioritize accordingly.
- ⛔ **AVOID:** Chasing every theoretical threat equally instead of protecting the highest-value, highest-likelihood assets.

### 2. Identity & access management
- ✅ **APPLY:** Enforce MFA, least privilege, and role-based access; separate duties for sensitive actions.
- ⛔ **AVOID:** Shared accounts and standing over-privileged access that make breaches total and untraceable.

### 3. Use the right crypto for the job
- ✅ **APPLY:** Hash with salted KDFs, encrypt in transit (TLS) and at rest, and manage keys properly.
- ⛔ **AVOID:** Home-grown crypto, weak/legacy algorithms, and hard-coded or unrotated keys.

### 4. Defense in depth + prepare to respond
- ✅ **APPLY:** Layer controls (network, host, app, data) and have an incident response plan: prepare, detect, contain, eradicate, recover.
- ⛔ **AVOID:** A single perimeter with no monitoring or IR plan — you won't detect or survive a breach.

## Cheat Reference — concepts to remember

- **Think in risk, not just threats** — Assess risk = likelihood × impact, then treat it (mitigate, transfer, accept, avoid) and prioritize accordingly.
- **Identity & access management** — Enforce MFA, least privilege, and role-based access; separate duties for sensitive actions.
- **Use the right crypto for the job** — Hash with salted KDFs, encrypt in transit (TLS) and at rest, and manage keys properly.
- **Defense in depth + prepare to respond** — Layer controls (network, host, app, data) and have an incident response plan: prepare, detect, contain, eradicate, recover.

## Interview Questions

#### Q1. What is the CIA triad?
Confidentiality, Integrity, and Availability — the three core goals of information security that guide control selection and trade-offs.

#### Q2. What is defense in depth?
Layering multiple, independent security controls (physical, network, host, application, data) so that if one fails, others still protect the asset.

#### Q3. Authentication vs authorization?
Authentication verifies who you are (passwords, MFA, biometrics); authorization determines what you're allowed to do (permissions, roles). Both are checked, server-side, on every request.

#### Q4. How do you handle a security incident?
Follow an IR lifecycle: preparation, identification/detection, containment, eradication, recovery, and lessons learned — to limit damage and prevent recurrence.

---

_Curated by Ali Arsalan · https://aliarsalan177.github.io · Generated from the Engineering Guides._


<hr>

