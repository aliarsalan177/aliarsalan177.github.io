# AGENTS — Engineering Guides (All Topics)

> 32 topics · Curated by Ali Arsalan · https://aliarsalan177.github.io/guides/

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

## Full Cheat Sheet — every concept

### Basic Types & Inference
- Annotate with `let name: string`, `age: number`, `ok: boolean`; TS infers types when obvious.
- Annotate function params and public return types; let locals be inferred.
- Special types: any (opt-out), unknown (safe top type — narrow first), never (impossible), void (no return).

### Arrays, Tuples, Enums
- Arrays: number[] or Array<number>.
- Tuples: fixed-length, mixed types — [string, number]; support destructuring.
- Enums: named constants — enum Color { Red, Green } (or prefer union literal types).
- Maps: new Map<K, V>().

### Unions, Intersections & Narrowing
- Union: A | B (either). Intersection: A & B (both).
- Literal unions model state: type Status = 'idle' | 'loading' | 'error'.
- Narrow with typeof, instanceof, `in`, equality, or custom type guards (x is Foo).
- Discriminated unions: a shared literal field lets the compiler exhaustively check each case.

### Interfaces vs Types
- interface — extendable object contracts; supports declaration merging.
- type alias — unions, tuples, mapped and conditional types.
- Optional (`?`), readonly, and index signatures ([key: string]: T) on both.

### Generics
- Generic functions: function first<T>(arr: T[]): T. Generic classes: class Stack<T> {}.
- Constrain with `T extends …`; provide defaults with `T = X`.
- Preserve input↔output type relationships instead of falling back to any.

### Utility & Meta Types
- Partial, Required, Readonly, Pick<T,K>, Omit<T,K>, Record<K,V>, ReturnType<F>, Parameters<F>, Awaited<T>.
- keyof T — union of a type's keys; typeof value — the type of a value.
- Mapped types ({ [K in keyof T]: … }) and conditional types (T extends U ? X : Y).

### Assertions, Classes & Config
- Assertions: value as Type (unchecked — avoid unless you're sure); non-null value! (use sparingly).
- Optional chaining a?.b?.c and nullish coalescing a ?? b.
- Classes: access modifiers public/private/protected, readonly, and constructor shorthand.
- Always compile with strict: true (plus noUncheckedIndexedAccess) for real safety.

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

## Full Cheat Sheet — every concept

### Components & Modules
- Component = template + encapsulated CSS + TS class; generate with `ng g c Name`.
- @NgModule: declarations, imports, exports, providers, bootstrap (or standalone components).

### Data Binding
- Interpolation {{ expr }}, property [prop]="v", event (event)="handler()", two-way [(ngModel)]="x".
- ngModel needs FormsModule.

### Directives & Pipes
- Structural (*ngIf, *ngFor, *ngSwitch) add/remove DOM — don't forget the asterisk.
- Attribute (ngClass, ngStyle) change appearance/behavior.
- Pipes transform in templates: {{ value | date }} / uppercase / currency.

### Services & DI
- @Injectable services + constructor injection; scope providers (root vs component).
- Provide before injecting; root providers are singletons.

### Reactivity & Performance
- RxJS Observables for async; prefer the | async pipe (auto-unsubscribe) or takeUntilDestroyed.
- OnPush change detection with immutable inputs / signals to minimize checks.
- Lifecycle hooks: ngOnInit, ngOnChanges, ngAfterViewInit, ngOnDestroy.

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

## Full Cheat Sheet — every concept

### Core Components
- View (≈ div), Text (all text must be inside it), Image, ScrollView, TextInput.
- Pressable / TouchableOpacity for touch via onPress.

### Styling & Layout
- Styles are JS objects (StyleSheet.create), camelCase props (backgroundColor).
- Flexbox is the layout system (flexDirection defaults to column); units are dp.
- No CSS files / no DOM.

### Lists & Performance
- FlatList / SectionList virtualize long lists (keyExtractor, getItemLayout).
- Don't map huge arrays inside ScrollView — it mounts everything.
- Memoize rows; animate with the native driver (Reanimated); keep the JS thread free.

### Platform & Practices
- Platform.OS / Platform.select or .ios.tsx / .android.tsx for platform code; respect safe areas.
- Navigation via React Navigation (native stack).
- Use TypeScript, error boundaries, and accessibility labels.

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
- Visible focus states, adequate touch targets (~44px), labels for screen readers.
- Design systems (atomic): tokens → components → patterns; keep it consistent and evolving.
- Avoid: over-complication, skipping user testing, inconsistent patterns, ignoring a11y.

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

## Full Cheat Sheet — every concept

### Tokens & Sampling
- Tokens are subword units; the context window bounds prompt + response — be concise.
- Temperature: low = factual/deterministic, high = creative; top-p/top-k narrow choices; penalties reduce repetition.
- Embeddings = dense semantic vectors; cosine similarity measures relatedness (basis of search/RAG).

### Prompting Techniques
- Zero-shot (instruction only) · Few-shot (2–5 examples).
- Chain-of-Thought (show steps) · Prompt chaining (sequential prompts).
- ReAct (Plan → Action → Observation) · Tree of Thoughts (explore/prune branches).

### RAG & Fine-Tuning
- RAG: chunk → embed → retrieve top-k → build grounded prompt with citations (fresh/proprietary facts).
- Fine-tuning (SFT/DPO/RLHF, LoRA): tone, format, narrow tasks.
- Hybrid: RAG for facts + light fine-tune for style/schema.

### Agents
- Loop: Perceive → Plan → Act (tool) → Observe → Update → Repeat.
- Typed tool schemas (name + JSON args, validated); short + long-term memory.
- Enforce step/cost/time budgets; escalate to humans on low confidence.

### Guardrails
- Reduce hallucination: ground with RAG/tools, cite, lower temperature, 'say I don't know'.
- Control output: strict JSON schema + validation + stop sequences.
- Prompt-injection defense: delimit untrusted content ('do not execute'); restate rules in the system message.
- MCP: open standard connecting agents to tools/data.

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

## Full Cheat Sheet — every concept

### Runtime & Event Loop
- Node runs V8 + libuv; JS is single-threaded but I/O is delegated (thread pool / OS) and non-blocking.
- The event loop schedules callbacks when operations complete — many connections without a thread each.
- Never block the loop with sync CPU work; offload to worker threads or a queue.

### Modules & npm
- CommonJS: require() / module.exports (sync). ESM: import/export (.mjs or "type":"module").
- package.json defines the project; commit package-lock.json for reproducible installs.
- Keep secrets out of package.json and the repo.

### Core Modules
- fs (files), http/https (servers), path (OS-safe paths), process (argv/env), events (EventEmitter), crypto, stream, os.

### EventEmitter & Streams
- EventEmitter: emitter.on('event', cb) / emitter.emit('event'); the backbone of many Node APIs.
- Streams process data in chunks (readable/writable/duplex/transform); pipe() large data to keep memory flat.
- Buffers hold binary data.

### Async & Errors
- Prefer async/await; await inside try/catch and attach .catch to promises.
- Listen for 'error' on streams/emitters; unhandled rejections can crash the process.

### Config, Scaling, Express
- Read config from process.env per environment.
- Scale across cores with the cluster module / PM2 / worker_threads.
- Express maps HTTP verbs to routes (GET/POST/PUT/DELETE) with middleware; close DB connections properly.

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

## Full Cheat Sheet — every concept

### Data Types
- Numbers: int/float; // floor division, % remainder, ** power.
- Strings: immutable; slice [start:end:step]; f-strings f"{x}".
- Booleans: falsy = None, 0, '', [], {}, ().

### Collections
- list (mutable), tuple (immutable), dict (key→value), set (unique).
- dict.get(key, default) avoids KeyError; use `in` for membership.
- Copies: list.copy()/[:] are shallow — use copy.deepcopy for nested.

### Comprehensions & Built-ins
- [x*2 for x in xs if x>0]; also set/dict comprehensions.
- Generators (yield) for lazy, memory-efficient iteration.
- Built-ins: map, filter, zip, enumerate, sorted, any, all, sum.

### Functions & Args
- Order: positional → *args → keyword-only → **kwargs.
- *args splats an iterable to positionals; **kwargs a dict to keywords.
- NEVER use a mutable default (def f(x, acc=[])) — use None and create inside.

### Advanced & OOP
- Decorators wrap functions (@decorator); closures capture enclosing scope.
- Classes: __init__, self, inheritance, @classmethod / @staticmethod, dunder methods.
- Scope is LEGB (Local → Enclosing → Global → Built-in); use global/nonlocal to rebind.

### Resources & Errors
- Context managers: `with open(...) as f:` guarantees cleanup.
- Catch specific exceptions (except ValueError:), not bare except.
- try/except/else/finally for full control.

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

## Full Cheat Sheet — every concept

### Ownership
- Every value has one owner; dropped when the owner leaves scope.
- Assigning/passing a non-Copy value moves it — the original is invalidated.
- Clone for a deep copy; Copy types (integers, bool) copy implicitly.

### Borrowing & Lifetimes
- &T immutable borrow, &mut T mutable borrow; lend without moving.
- Rule: many & OR one &mut at a time — never both.
- Lifetimes ('a) ensure references never outlive their data (no dangling).

### Types & Mutability
- Bindings immutable by default; opt in with `mut`.
- Structs group data (impl blocks add methods); enums are one-of variants with data.
- Pattern matching with `match` is exhaustive (compiler-checked).

### Traits & Generics
- Traits define shared behavior (like interfaces); implement for any type.
- Generics with trait bounds (T: Display) — monomorphized at compile time.
- Composition over inheritance (Rust has no class inheritance).

### Errors & Tooling
- Option<T> for maybe-present; Result<T,E> for fallible; no null, no exceptions.
- Propagate errors with `?`; avoid unwrap()/expect() in real paths (they panic).
- Cargo: cargo build / run / test / check; the borrow checker enforces safety at compile time.

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

## Full Cheat Sheet — every concept

### Structure & Types
- SPDX license → pragma version → imports → state variables → functions.
- Types: bool, uint/int (256-bit), address (use .call{value:} not .transfer), bytes32, mapping, struct, enum, arrays.

### Visibility & Functions
- public (auto getter), private, internal (+ inheritors), external (outside only).
- view (reads state), pure (neither), payable (receives Ether via msg.value).
- constant/immutable; modifiers enforce preconditions.

### Security
- Checks-Effects-Interactions + reentrancy guard (update state before external calls).
- Auth with msg.sender, never tx.origin (phishable).
- storage (persistent, costly) vs memory (temporary).

### Events, Errors, Gas
- Emit events (index up to 3 params) for cheap off-chain lookups.
- Custom errors (revert NotOwner()) over strings; require/assert/revert.
- Gas: minimize storage writes, avoid unbounded loops, unchecked{} for safe math.

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

## Full Cheat Sheet — every concept

### Big-O
- Time & space growth: O(1) < O(log n) < O(n) < O(n log n) < O(n²) < O(2ⁿ) < O(n!).
- Analyze worst case; watch nested loops (O(n²)) and hidden costs.

### Core Structures
- Array: O(1) index, O(n) search/insert-middle.
- Hash table: O(1) avg lookup/insert/delete; O(n) worst (collisions).
- Linked list: O(1) prepend/append, O(n) lookup. Stack (LIFO) / Queue (FIFO): O(1) ends.

### Trees, Heaps, Graphs
- BST: O(log n) avg, O(n) worst (unbalanced); AVL/Red-Black self-balance.
- Heap: priority queue, O(log n) ops.
- Graphs: adjacency list/matrix; BFS (queue, shortest path) & DFS (stack/recursion), both O(V+E).

### Algorithms
- Sorting: bubble/selection/insertion O(n²); merge O(n log n) stable; quick O(n log n) avg in-place.
- Searching: linear O(n); binary O(log n) on sorted data.

### Patterns
- Two pointers (sorted arrays), sliding window (substrings/subarrays).
- BFS/DFS (graphs/trees), recursion (watch stack depth).
- Dynamic programming: overlapping subproblems + memoization/tabulation.

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

## Full Cheat Sheet — every concept

### Principles
- CIA triad: Confidentiality, Integrity, Availability.
- Least privilege, defense in depth, assume breach, separation of duties.

### Threats & Attacks
- Phishing/social engineering, malware, DDoS, MITM, data breach.
- Web: SQL injection & XSS (untrusted input treated as code), CSRF.

### Identity & Crypto
- Authentication (who) with MFA vs authorization (what) — check both server-side.
- Symmetric (shared key, bulk) vs asymmetric (key pair, exchange/signatures); TLS combines both.
- Hash passwords with salted bcrypt/argon2; encrypt in transit & at rest; never roll your own crypto.

### Defense & Response
- Firewalls, VPN, IDS/IPS, antivirus, WAF.
- Validate/sanitize input; parameterized queries; output encoding; CSP.
- Incident response: prepare → detect → contain → eradicate → recover → learn.

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

## Full Cheat Sheet — every concept

### Host Discovery
- nmap <ip>; ping sweep `-sn`; targets: ranges (1-100), subnets (/24), lists (-iL file).

### Scan Types
- -sS SYN (stealth, default TCP), -sT full connect (detectable), -sU UDP.
- Ports: -p 80,443 / -p 1-100 / -p- (all 65535) / -F (top 100).

### Detection
- -sV service/version, -O OS (needs 1 open + 1 closed port), -A aggressive (all, noisy).
- NSE scripts: --script <category> (needs sudo; verify findings).

### Timing, Output, Ethics
- Timing -T0..-T5 (paranoid→insane); slower = stealthier.
- Output: -oN file.txt (normal), -oX (XML), -oG (grepable).
- Only scan systems you own or have written authorization to test.

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

## Full Cheat Sheet — every concept

### Navigation & Files
- ls (-l -a -h), cd (~ home, - previous), pwd.
- mkdir -p, cp -r, mv -i, rm -r (rm -rf forces), touch.

### Permissions
- chmod symbolic or octal (r=4 w=2 x=1): 755 dirs/execs, 644 files.
- chown user:group file; least privilege — avoid chmod 777.

### Viewing & Searching
- cat, less/more, head/tail; grep (-i -n -r); find by name/size/type.

### Processes & Services
- ps aux, top; kill <pid> (SIGTERM), kill -9 last resort.
- systemctl start/stop/restart/enable; schedule with cron (crontab -e).

### Pipes, Redirection, Net
- | pipe; > overwrite, >> append, 2>&1 merge stderr.
- ssh, scp, curl/wget; apt install/update; man <cmd> for docs.

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

## Full Cheat Sheet — every concept

### Types of Analytics
- Descriptive (what happened) · Diagnostic (why) · Predictive (what's next) · Prescriptive (what to do).

### Statistics
- Central tendency: mean (symmetric), median (robust to skew), mode (categorical).
- Dispersion: standard deviation vs the mean; correlation ∈ [-1, +1].
- Check distribution/normality before applying tests.

### Inference & Causality
- Hypothesis tests: t-test (continuous), chi-square (categorical); confidence intervals.
- Regression: linear (continuous), logistic (binary).
- Correlation ≠ causation — establish causality with A/B tests, diff-in-diff, matching.

### Segmentation & Modeling
- RFM (recency/frequency/monetary), clustering, cohorts, funnels.
- Random forest (ensemble), time-series forecasting (e.g. Prophet).

### Practice
- Define a few actionable KPIs; clean data first; visualize clearly.
- Avoid vanity metrics and cluttered dashboards.

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

## Full Cheat Sheet — every concept

### Concurrency
- Goroutines: `go fn()` — lightweight Go-managed threads; main doesn't wait automatically.
- Channels: ch <- v send, <-ch receive; unbuffered blocks until received; buffered holds capacity.
- sync.WaitGroup (Add/Done/Wait) to wait; `select` to multiplex channels.
- sync.Mutex (Lock/defer Unlock) to guard shared state; avoid concurrent map writes (data race).

### Data
- Arrays fixed ([3]int); slices dynamic ([]int) over an array — append() to grow.
- Maps: make(map[K]V); comma-ok: v, ok := m[k].
- Structs group fields; capitalized names are exported (public).

### Interfaces & Types
- Interfaces are satisfied implicitly — implement the methods, no `implements`.
- Compose via embedding; don't mix pointer and value receivers on one type.
- Zero values: 0, false, "", nil (uninitialized is usable).

### Errors & Idioms
- No exceptions: return (value, error) and check err immediately.
- Wrap with fmt.Errorf("...: %w", err); custom errors implement Error() string.
- defer runs on function return (LIFO) for cleanup; all args are passed by value (use pointers to mutate).

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

## Full Cheat Sheet — every concept

### Types & Memory
- Value types (int, struct, enum) store data directly, copied on assignment.
- Reference types (class, string, array) store a heap reference, shared on assignment.
- records give value-based equality + immutability; `with` for non-destructive copies.

### Classes & Members
- Classes = blueprints; auto-properties { get; set; }; partial classes split across files.
- Interfaces define contracts; inheritance + polymorphism via virtual/override.
- Access: public/private/protected/internal.

### Generics & Collections
- Generics <T> for reusable, type-safe code without boxing.
- Arrays (fixed), List<T> (dynamic), Dictionary<K,V> (key→value).
- switch requires break/return per case (no implicit fall-through).

### Async & LINQ
- async methods return Task/Task<T>; await frees the thread; don't block with .Result/.Wait().
- LINQ: declarative queries (Where/Select/OrderBy); deferred until enumerated (ToList/Count).
- Delegates & events model callbacks/pub-sub.

### Safety & Errors
- Enable nullable reference types (#nullable enable) to catch null derefs at compile time.
- Catch specific exceptions before general ones; use try/catch/finally.
- Prefer dependency injection for loose coupling.

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

## Full Cheat Sheet — every concept

### Query Basics
- SELECT cols FROM table WHERE filter ORDER BY col ASC/DESC LIMIT n.
- Order of evaluation: FROM → WHERE → GROUP BY → HAVING → SELECT → ORDER BY → LIMIT.
- DISTINCT removes duplicate rows.

### Grouping & Aggregates
- GROUP BY collapses rows; aggregates: COUNT, SUM, AVG, MIN, MAX (ignore NULLs).
- WHERE filters rows (no aggregates); HAVING filters groups (aggregates allowed).

### Joins
- INNER JOIN = matches in both; LEFT/RIGHT JOIN = all of one side + matches (NULLs otherwise).
- FULL OUTER JOIN = all rows from both.
- Always specify the ON condition — a missing one causes a cartesian product.

### Structure & Integrity
- Primary key uniquely identifies a row; foreign key links tables.
- Normalization removes redundancy; indexes speed reads (cost writes).
- Transactions are ACID (Atomic, Consistent, Isolated, Durable).

### Gotchas
- NULL: use IS NULL / IS NOT NULL, and COALESCE() to default.
- UNION dedupes; UNION ALL keeps duplicates.
- Subqueries run first and feed the outer query.

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

## Full Cheat Sheet — every concept

### Formulas & References
- SUM, AVERAGE, IF, COUNTIF, SUMIF; combine IF + AND/OR.
- Relative (A1) shift on copy; absolute ($A$1) stay fixed; mixed ($A1 / A$1); F4 cycles.

### Lookups
- Prefer XLOOKUP or INDEX+MATCH (any direction, survive column moves).
- VLOOKUP looks right only and breaks on inserted columns.

### Robustness & Analysis
- Wrap risky formulas in IFERROR; watch #DIV/0!, #N/A, #NAME?, #VALUE!, #REF!.
- Pivot tables summarize/cross-tab large data; charts to visualize.
- Data validation + conditional formatting for clean, readable sheets.

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

## Full Cheat Sheet — every concept

### Risk & Governance
- CIA triad; risk = likelihood × impact; treat by mitigate/transfer/accept/avoid.
- Policies, standards, compliance (GDPR/PCI/HIPAA), security awareness.

### Identity & Access
- AAA (authentication, authorization, accounting); MFA; RBAC; least privilege; separation of duties.

### Cryptography
- Symmetric (AES) vs asymmetric (RSA/ECC); hashing (SHA-2) + salted KDFs for passwords.
- PKI, certificates, TLS; proper key management/rotation.

### Network & Threats
- Firewalls, VPN, IDS/IPS, segmentation, zero trust.
- Attacks: phishing, malware, MITM, DoS/DDoS, injection; defense in depth.

### Operations
- Vulnerability assessment & pentesting; patch management.
- Incident response lifecycle; logging/monitoring; backups & disaster recovery.

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

# Agent Guide — PHP (Modern / Vanilla)

> Modern PHP 8+ is typed, fast and Composer-driven. Write strict, secure, PSR-compliant code — not the legacy stuff of old.
> Category: Languages · Source: https://aliarsalan177.github.io/guides/php/

## Purpose

A field guide for **AI coding agents** (and engineers): the concepts to apply and the
mistakes to avoid when building with PHP (Modern / Vanilla). Load this into your agent's context so it
keeps these concepts in mind and does **not** repeat these mistakes.

## Rules — Apply / Avoid

### 1. Write typed, strict modern PHP
- ✅ **APPLY:** Add declare(strict_types=1), type-hint params/returns, use enums, readonly props, and named arguments (PHP 8+).
- ⛔ **AVOID:** Loose, untyped legacy style with `@` error suppression and implicit type juggling.

### 2. Never trust input — escape by context
- ✅ **APPLY:** Validate/filter input; use prepared statements (PDO/mysqli) for SQL; htmlspecialchars() on output.
- ⛔ **AVOID:** String-concatenated SQL and echoing raw user input — SQL injection and XSS.

### 3. Use Composer + PSR standards
- ✅ **APPLY:** Manage deps with Composer, autoload via PSR-4, follow PSR-12 style; don't reinvent libraries.
- ⛔ **AVOID:** Manual require chains and copy-pasted vendor code with no autoloading or dependency management.

### 4. Handle errors with exceptions
- ✅ **APPLY:** Throw/catch typed exceptions; log server-side; return safe messages to users.
- ⛔ **AVOID:** Suppressing errors with @, exposing stack traces, or leaving display_errors on in production.

### 5. Hash secrets and manage config
- ✅ **APPLY:** password_hash()/password_verify() (bcrypt/argon2); keep secrets in env, not code.
- ⛔ **AVOID:** md5/sha1 for passwords, hard-coded credentials, and committing .env.

## Cheat Reference — concepts to remember

- **Write typed, strict modern PHP** — Add declare(strict_types=1), type-hint params/returns, use enums, readonly props, and named arguments (PHP 8+).
- **Never trust input — escape by context** — Validate/filter input; use prepared statements (PDO/mysqli) for SQL; htmlspecialchars() on output.
- **Use Composer + PSR standards** — Manage deps with Composer, autoload via PSR-4, follow PSR-12 style; don't reinvent libraries.
- **Handle errors with exceptions** — Throw/catch typed exceptions; log server-side; return safe messages to users.
- **Hash secrets and manage config** — password_hash()/password_verify() (bcrypt/argon2); keep secrets in env, not code.

## Full Cheat Sheet — every concept

### Language Basics
- declare(strict_types=1); scalar types (int, float, string, bool), arrays, objects, null.
- Variables $x; arrays (indexed & associative); string interpolation "$x" and heredoc.
- == juggles types, === strict; the spaceship <=> for comparison.

### Modern PHP 8+
- Constructor property promotion, readonly properties, enums, union/intersection types.
- match (strict, returns a value), nullsafe ?->, named args, first-class callable syntax.
- Attributes (#[Route(...)]); the JIT compiler.

### OOP
- class/interface/trait/abstract; visibility public/protected/private; static; final.
- Namespaces + PSR-4 autoloading via Composer.
- Exceptions: try/catch/finally, custom exception classes.

### Security (critical)
- SQL: PDO prepared statements with bound params; whitelist dynamic identifiers.
- XSS: htmlspecialchars() on output; CSRF tokens on forms.
- Passwords: password_hash()/password_verify(); never unserialize()/eval()/extract() untrusted data.
- Config: env vars for secrets; APP debug off in prod; file perms tight.

### Tooling & Standards
- Composer (deps + autoload), PSR-4/PSR-12; PHPUnit/Pest for tests.
- Static analysis (PHPStan/Psalm), formatting (PHP-CS-Fixer).

## Interview Questions

#### Q1. == vs === in PHP?
== compares with type juggling (0 == 'a' was true before PHP 8); === compares value and type with no coercion. Prefer === to avoid surprising conversions (PHP 8 also fixed many juggling quirks).

#### Q2. How do you prevent SQL injection in PHP?
Use prepared statements with bound parameters via PDO or mysqli, never concatenate user input into SQL. Validate and whitelist anything that can't be bound (like column names).

#### Q3. What are PSR standards and Composer?
PSRs are PHP-FIG recommendations (PSR-4 autoloading, PSR-12 coding style, PSR-7 HTTP messages). Composer is the dependency manager that autoloads packages and your code per PSR-4.

#### Q4. What's new in modern PHP (8.x)?
Typed properties, union types, enums, readonly properties, named arguments, match expressions, nullsafe operator (?->), constructor property promotion, attributes, and the JIT compiler.

#### Q5. How should passwords be stored?
With password_hash() (bcrypt/argon2, salted and slow) and verified with password_verify() — never md5/sha1 or plaintext.

---

_Curated by Ali Arsalan · https://aliarsalan177.github.io · Generated from the Engineering Guides._


<hr>

# Agent Guide — Laravel

> Batteries-included PHP framework: Eloquent, Blade, queues, and elegant APIs. Lean on the conventions and its built-in security (aligned with OWASP).
> Category: Backend & Infra · Source: https://aliarsalan177.github.io/guides/laravel/

## Purpose

A field guide for **AI coding agents** (and engineers): the concepts to apply and the
mistakes to avoid when building with Laravel. Load this into your agent's context so it
keeps these concepts in mind and does **not** repeat these mistakes.

## Rules — Apply / Avoid

### 1. Guard against mass assignment
- ✅ **APPLY:** Fill models from $request->validated() or ->only([...]); set $fillable explicitly.
- ⛔ **AVOID:** Model::create($request->all()) with empty $guarded / unguarded models — attackers can set any column.

### 2. Query safely with Eloquent
- ✅ **APPLY:** Use Eloquent/Query Builder (parameterized by default); for raw SQL, bind values: whereRaw('x = ?', [$v]).
- ⛔ **AVOID:** Interpolating input into raw queries or letting users choose column names without a whitelist.

### 3. Escape output & keep CSRF on
- ✅ **APPLY:** Use Blade {{ }} (auto-escapes) and @csrf on POST forms; keep VerifyCsrfToken on web routes.
- ⛔ **AVOID:** {!! !!} on untrusted data (XSS) and disabling CSRF except for stateless APIs/webhooks.

### 4. Authorize with policies/gates
- ✅ **APPLY:** Centralize permissions in Policies/Gates; use Sanctum (SPA/tokens) or Passport (OAuth2) for APIs.
- ⛔ **AVOID:** Ad-hoc permission checks scattered in controllers, or trusting the client.

### 5. Harden config & production
- ✅ **APPLY:** APP_DEBUG=false in prod, run key:generate, encrypt cookies, set secure/httpOnly/sameSite, throttle routes, add security headers.
- ⛔ **AVOID:** Debug mode on in production, committing .env, and unbounded (unthrottled) endpoints.

### 6. Offload slow work to queues
- ✅ **APPLY:** Dispatch jobs to queues (mail, images, webhooks); use events/listeners and the scheduler.
- ⛔ **AVOID:** Doing slow work inside the request/response cycle and blocking users.

## Cheat Reference — concepts to remember

- **Guard against mass assignment** — Fill models from $request->validated() or ->only([...]); set $fillable explicitly.
- **Query safely with Eloquent** — Use Eloquent/Query Builder (parameterized by default); for raw SQL, bind values: whereRaw('x = ?', [$v]).
- **Escape output & keep CSRF on** — Use Blade {{ }} (auto-escapes) and @csrf on POST forms; keep VerifyCsrfToken on web routes.
- **Authorize with policies/gates** — Centralize permissions in Policies/Gates; use Sanctum (SPA/tokens) or Passport (OAuth2) for APIs.
- **Harden config & production** — APP_DEBUG=false in prod, run key:generate, encrypt cookies, set secure/httpOnly/sameSite, throttle routes, add security headers.
- **Offload slow work to queues** — Dispatch jobs to queues (mail, images, webhooks); use events/listeners and the scheduler.

## Full Cheat Sheet — every concept

### MVC & Routing
- Routes (web.php/api.php) → Controllers → Views (Blade) / JSON resources.
- Route model binding; middleware groups (web, api); resource controllers.

### Eloquent ORM
- Models, migrations, factories, seeders; relationships (hasMany, belongsTo, morphTo).
- $fillable/$guarded for mass assignment; eager load with with() to avoid N+1.
- Query Builder + Eloquent are parameterized; bind raw queries.

### Requests & Validation
- Form Requests / $request->validate(); use validated()/only(), not all().
- Blade {{ }} escapes; {!! !!} only for trusted HTML.

### Auth & Authorization
- Starter kits: Breeze, Fortify, Jetstream. APIs: Sanctum / Passport.
- Gates & Policies for authorization; guards (session/token) + providers (eloquent/database).

### Security (OWASP)
- APP_DEBUG=false; key:generate; EncryptCookies; secure/httpOnly/sameSite cookies.
- CSRF (@csrf, VerifyCsrfToken); throttle middleware (rate limiting); validate uploads + basename().
- Security headers (CSP, HSTS, X-Frame-Options); never unserialize/eval/extract untrusted data.

### Async & Ecosystem
- Queues & Jobs, Events/Listeners, Task Scheduler; Cache, Broadcasting, Notifications.
- Artisan CLI; Tinker; Telescope/Horizon; Pest/PHPUnit tests.

## Interview Questions

#### Q1. How does Laravel prevent mass-assignment vulnerabilities?
Models restrict which attributes can be bulk-assigned via $fillable (allowlist) or $guarded (blocklist). Best practice is to fill from validated input ($request->validated()) rather than $request->all().

#### Q2. How does Blade protect against XSS?
{{ $var }} auto-escapes output with htmlspecialchars. Only {!! $var !!} outputs raw HTML — use it exclusively for trusted content, never untrusted user input.

#### Q3. Sanctum vs Passport?
Sanctum is lightweight token/SPA cookie auth for first-party apps and simple API tokens; Passport is a full OAuth2 server for third-party API access. Pick Sanctum unless you need OAuth2 flows.

#### Q4. What is Eloquent and how does it avoid SQL injection?
Eloquent is Laravel's ActiveRecord ORM; it and the Query Builder use PDO prepared statements with bound parameters by default, so ordinary queries are injection-safe. Raw queries must bind values explicitly.

#### Q5. Key production hardening steps in Laravel?
APP_DEBUG=false, generate APP_KEY, keep CSRF + EncryptCookies middleware, set secure cookie flags, apply throttle middleware, validate file uploads, and add security headers (CSP, HSTS, X-Frame-Options).

---

_Curated by Ali Arsalan · https://aliarsalan177.github.io · Generated from the Engineering Guides._


<hr>

# Agent Guide — Magento 2

> Enterprise e-commerce on a modular, DI-driven PHP architecture. Extend the right way — plugins and DI, never core edits.
> Category: Backend & Infra · Source: https://aliarsalan177.github.io/guides/magento/

## Purpose

A field guide for **AI coding agents** (and engineers): the concepts to apply and the
mistakes to avoid when building with Magento 2. Load this into your agent's context so it
keeps these concepts in mind and does **not** repeat these mistakes.

## Rules — Apply / Avoid

### 1. Never edit core; extend via modules
- ✅ **APPLY:** Build custom modules; change behavior with plugins (interceptors), preferences, and events/observers.
- ⛔ **AVOID:** Editing vendor/magento core files — upgrades wipe them and it breaks the platform.

### 2. Use dependency injection, not ObjectManager
- ✅ **APPLY:** Inject dependencies via the constructor and di.xml; use factories/proxies for non-injectables.
- ⛔ **AVOID:** Calling ObjectManager::getInstance() directly in business code — it hides deps and is an anti-pattern.

### 3. Prefer plugins over rewrites
- ✅ **APPLY:** Use before/after/around plugins to modify public methods without replacing whole classes.
- ⛔ **AVOID:** Class preferences (rewrites) when a plugin would do — rewrites collide and are fragile across modules.

### 4. Respect service contracts & indexing/cache
- ✅ **APPLY:** Program against Api/ service-contract interfaces and repositories; keep indexers and cache in mind.
- ⛔ **AVOID:** Direct SQL/resource-model hacks, and forgetting to reindex / flush cache after data or config changes.

### 5. Optimize for performance
- ✅ **APPLY:** Use Varnish full-page cache, Redis (session/cache), OpenSearch, production mode, and flat/async indexing.
- ⛔ **AVOID:** Running developer mode in production and uncached, unindexed catalog queries at scale.

## Cheat Reference — concepts to remember

- **Never edit core; extend via modules** — Build custom modules; change behavior with plugins (interceptors), preferences, and events/observers.
- **Use dependency injection, not ObjectManager** — Inject dependencies via the constructor and di.xml; use factories/proxies for non-injectables.
- **Prefer plugins over rewrites** — Use before/after/around plugins to modify public methods without replacing whole classes.
- **Respect service contracts & indexing/cache** — Program against Api/ service-contract interfaces and repositories; keep indexers and cache in mind.
- **Optimize for performance** — Use Varnish full-page cache, Redis (session/cache), OpenSearch, production mode, and flat/async indexing.

## Full Cheat Sheet — every concept

### Module Structure
- app/code/Vendor/Module with registration.php + etc/module.xml.
- etc/di.xml (DI, plugins, preferences), events.xml, acl.xml, adminhtml/frontend areas.
- Model/ ResourceModel/ Api/ Block/ Controller/ view/ Setup or declarative schema (db_schema.xml).

### Dependency Injection
- Constructor injection; di.xml for preferences, plugins, virtual types.
- Factories for non-injectable (entity) objects; Proxies for lazy/expensive deps.
- Never use ObjectManager in business logic.

### Extension Points
- Plugins (before/after/around) on public methods; Observers on events; Preferences (rewrite, last resort).
- UI Components (grids/forms) via XML; layout XML + templates (.phtml) + ViewModels.

### Data & APIs
- Service contracts (Api/ interfaces) + repositories; data interfaces.
- REST & GraphQL; declarative schema (db_schema.xml) + data/schema patches.

### Performance & Ops
- Modes: default/developer/production; bin/magento setup:di:compile, setup:static-content:deploy.
- Varnish FPC, Redis cache/session, OpenSearch; indexers (bin/magento indexer:reindex); cache:flush.

## Interview Questions

#### Q1. How do you customize Magento 2 without touching core?
Create a custom module and change behavior with plugins (interceptors), event observers, preferences (class rewrites as a last resort), and configuration via di.xml/XML — so upgrades don't overwrite your changes.

#### Q2. Plugin (interceptor) vs preference?
A plugin wraps a public method (before/after/around) to modify inputs, outputs, or flow without replacing the class, so multiple modules can coexist. A preference replaces the whole class implementation and can conflict — use it only when a plugin can't.

#### Q3. Why avoid ObjectManager directly?
It hides dependencies, breaks testability, and bypasses Magento's DI. Inject dependencies through the constructor (resolved via di.xml); use factories for objects you must create at runtime.

#### Q4. What are service contracts?
Api/ interfaces (data interfaces + service interfaces like repositories) that define a stable public API for a module, decoupling callers from implementation and enabling web APIs (REST/GraphQL).

#### Q5. Key Magento 2 performance levers?
Production mode, Varnish full-page cache, Redis for cache/sessions, OpenSearch/Elasticsearch, proper indexing (schedule/async), composer autoloader optimization, and a CDN for static/media.

---

_Curated by Ali Arsalan · https://aliarsalan177.github.io · Generated from the Engineering Guides._


<hr>

# Agent Guide — Next.js (App Router)

> React framework with the App Router, Server Components, and file-based routing. Default to the server, fetch on the server, ship less JS.
> Category: Frontend · Source: https://aliarsalan177.github.io/guides/nextjs/

## Purpose

A field guide for **AI coding agents** (and engineers): the concepts to apply and the
mistakes to avoid when building with Next.js (App Router). Load this into your agent's context so it
keeps these concepts in mind and does **not** repeat these mistakes.

## Rules — Apply / Avoid

### 1. Server Components by default
- ✅ **APPLY:** Keep components server-rendered; add 'use client' only where you need state, effects, or browser APIs.
- ⛔ **AVOID:** Slapping 'use client' at the top of the tree — it ships everything to the browser and kills the benefit.

### 2. Fetch data on the server
- ✅ **APPLY:** await data directly in server components; use fetch caching / revalidate and Suspense for streaming.
- ⛔ **AVOID:** useEffect fetch waterfalls for data you could load on the server; leaking secrets into client bundles.

### 3. Use the file-based conventions
- ✅ **APPLY:** Organize with app/ segments: layout, page, loading, error, route handlers; colocate components.
- ⛔ **AVOID:** Fighting the router with ad-hoc structures; putting server-only code in client components.

### 4. Mutate with Server Actions / Route Handlers
- ✅ **APPLY:** Use server actions or route handlers for mutations; revalidatePath/Tag to refresh cached data.
- ⛔ **AVOID:** Building throwaway API endpoints for everything when a server action fits; forgetting to revalidate.

### 5. Optimize assets and metadata
- ✅ **APPLY:** next/image, next/font, and the Metadata API (+ generateStaticParams for static export).
- ⛔ **AVOID:** Unoptimized <img>, layout-shifting fonts, and missing SEO metadata.

## Cheat Reference — concepts to remember

- **Server Components by default** — Keep components server-rendered; add 'use client' only where you need state, effects, or browser APIs.
- **Fetch data on the server** — await data directly in server components; use fetch caching / revalidate and Suspense for streaming.
- **Use the file-based conventions** — Organize with app/ segments: layout, page, loading, error, route handlers; colocate components.
- **Mutate with Server Actions / Route Handlers** — Use server actions or route handlers for mutations; revalidatePath/Tag to refresh cached data.
- **Optimize assets and metadata** — next/image, next/font, and the Metadata API (+ generateStaticParams for static export).

## Full Cheat Sheet — every concept

### App Router Files
- app/segment/: layout.tsx, page.tsx, loading.tsx, error.tsx, not-found.tsx, route.ts, template.tsx.
- Dynamic segments [id], catch-all [...slug], route groups (group), private _folder.
- generateStaticParams + generateMetadata for static/SEO.

### Rendering
- Server Components default (no JS shipped); 'use client' for interactivity.
- Streaming with Suspense + loading.js; partial prerendering.
- SSG / SSR / ISR (revalidate); output: 'export' for fully static sites.

### Data & Mutations
- await fetch in server components; caching via { cache, next: { revalidate, tags } }.
- Server Actions ('use server') for mutations; Route Handlers (route.ts) for APIs.
- revalidatePath / revalidateTag to refresh cached data.

### Optimization & Config
- next/image (responsive, lazy), next/font (no layout shift), next/script.
- Metadata API + JSON-LD; Middleware (edge) for auth/redirects.
- Env: NEXT_PUBLIC_ exposed to client; everything else server-only.

## Interview Questions

#### Q1. Server Components vs Client Components?
Server Components render on the server with zero JS shipped and can fetch data/secrets directly; Client Components ('use client') run in the browser for interactivity (state, effects, events). Default to server, opt into client at the leaves.

#### Q2. How does data fetching work in the App Router?
You await data directly in async server components. fetch is cached by default with control via { cache, next: { revalidate, tags } }; Suspense + loading.js stream UI while data loads.

#### Q3. What are Server Actions?
Async functions marked 'use server' that run on the server and can be called from components/forms to mutate data without hand-writing an API route, then revalidatePath/revalidateTag to refresh caches.

#### Q4. SSG vs SSR vs ISR?
Static Generation prerenders at build (fast, cacheable); SSR renders per request (fresh, dynamic); Incremental Static Regeneration rebuilds static pages on a revalidate interval — best of both for content that changes occasionally.

#### Q5. How do special files work (layout, page, loading, error)?
Each route segment can have layout.tsx (shared UI/state across children), page.tsx (the route UI), loading.tsx (Suspense fallback), error.tsx (error boundary), and route.ts (API handler).

---

_Curated by Ali Arsalan · https://aliarsalan177.github.io · Generated from the Engineering Guides._


<hr>

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


<hr>

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


<hr>

# Agent Guide — System Design & Architecture

> Designing systems that scale, plus the everyday architecture that keeps a codebase sane: folder structure, naming, DB and API design.
> Category: Architecture & Systems · Source: https://aliarsalan177.github.io/guides/system-design/

## Purpose

A field guide for **AI coding agents** (and engineers): the concepts to apply and the
mistakes to avoid when building with System Design & Architecture. Load this into your agent's context so it
keeps these concepts in mind and does **not** repeat these mistakes.

## Rules — Apply / Avoid

### 1. Design from requirements & constraints
- ✅ **APPLY:** Clarify functional + non-functional needs (scale, latency, consistency), estimate load, then choose components.
- ⛔ **AVOID:** Jumping to a microservices/Kafka diagram before you understand the actual requirements and scale.

### 2. Scale with statelessness, caching & queues
- ✅ **APPLY:** Keep app servers stateless (scale horizontally behind a load balancer), cache hot reads, and use queues to smooth spikes.
- ⛔ **AVOID:** Sticky server state, hammering the DB for every read, and doing slow work synchronously in the request path.

### 3. Model data for access patterns
- ✅ **APPLY:** Normalize for integrity, denormalize/index for read performance; pick SQL vs NoSQL by consistency & query needs.
- ⛔ **AVOID:** One-size-fits-all schemas, missing indexes on filtered columns, and N+1 queries.

### 4. Consistent, predictable structure & naming
- ✅ **APPLY:** Organize by feature/domain, colocate related files, and name consistently (PascalCase components, kebab-case files/routes, plural table names).
- ⛔ **AVOID:** Mixing casing conventions, dumping everything in one folder, and vague names (utils.js, data.ts, Manager).

### 5. Design clear API contracts
- ✅ **APPLY:** Use consistent REST resources (or GraphQL schema), proper status codes, versioning, pagination, and validation.
- ⛔ **AVOID:** Chatty, inconsistent endpoints, leaking internal models, and breaking changes with no versioning.

## Cheat Reference — concepts to remember

- **Design from requirements & constraints** — Clarify functional + non-functional needs (scale, latency, consistency), estimate load, then choose components.
- **Scale with statelessness, caching & queues** — Keep app servers stateless (scale horizontally behind a load balancer), cache hot reads, and use queues to smooth spikes.
- **Model data for access patterns** — Normalize for integrity, denormalize/index for read performance; pick SQL vs NoSQL by consistency & query needs.
- **Consistent, predictable structure & naming** — Organize by feature/domain, colocate related files, and name consistently (PascalCase components, kebab-case files/routes, plural table names).
- **Design clear API contracts** — Use consistent REST resources (or GraphQL schema), proper status codes, versioning, pagination, and validation.

## Full Cheat Sheet — every concept

### Scalability Building Blocks
- Load balancer + stateless app servers (horizontal scale); vertical vs horizontal scaling.
- Caching layers: CDN (edge), Redis/Memcached (app), DB query cache; cache invalidation strategy.
- Async: message queues (SQS/Kafka/RabbitMQ), workers, event-driven; rate limiting & backpressure.

### Data & DB Design
- Normalization (1NF–3NF) for integrity; denormalize + index for reads.
- Primary/foreign keys, indexes on filtered/joined columns; avoid N+1.
- Scaling: replication (read replicas), sharding/partitioning; SQL vs NoSQL by consistency & pattern.
- Transactions & ACID vs BASE/eventual consistency; CAP trade-offs.

### Architecture Styles
- Monolith (simple, one deploy) → modular monolith → microservices (independent scale/deploy, more ops).
- Layered / hexagonal / clean architecture; separation of concerns; one-way dependencies.
- Sync (REST/GraphQL/gRPC) vs async (events); API gateway; BFF.

### File / Folder & Naming
- Feature/domain-based folders; colocate component + style + test + types.
- Naming: PascalCase components/classes, camelCase vars/functions, kebab-case files/routes/URLs, plural DB tables, snake_case columns.
- index files for public API of a module; keep imports flowing inward.

### API Design
- REST: resource nouns, proper verbs + status codes, pagination, filtering, versioning (/v1).
- GraphQL: typed schema, resolvers, avoid over/under-fetching; validate all input.
- Idempotency, error format consistency, auth (OAuth2/JWT), rate limits.

### Reliability
- Redundancy & failover, health checks, graceful degradation, circuit breakers, retries with backoff.
- Observability: logs, metrics, traces, alerts; SLAs/SLOs.

## Interview Questions

#### Q1. How do you approach a system design question?
Clarify requirements (functional + non-functional), estimate scale (QPS, data size), define the API and data model, sketch the high-level architecture (LB, app tier, cache, DB, queue), then dig into bottlenecks, trade-offs, and failure modes.

#### Q2. How do you scale a read-heavy system?
Add caching (CDN, Redis), read replicas, and denormalized read models; keep app servers stateless behind a load balancer; use pagination and indexes. Introduce queues/async for writes and heavy work.

#### Q3. SQL vs NoSQL — how do you choose?
SQL for strong consistency, relations, and complex queries/transactions; NoSQL (document/key-value/wide-column) for massive scale, flexible schemas, and simple access patterns. Match the store to the access pattern; polyglot persistence is common.

#### Q4. What's the CAP theorem?
Under a network partition, a distributed system can guarantee at most two of Consistency, Availability, Partition tolerance — since partitions happen, you effectively trade consistency vs availability (CP vs AP).

#### Q5. How should you structure a project's files/folders?
Prefer feature/domain-based grouping (colocate component, styles, tests, logic) over type-based once the app grows, keep a consistent naming convention, and enforce clear module boundaries so dependencies flow one way.

---

_Curated by Ali Arsalan · https://aliarsalan177.github.io · Generated from the Engineering Guides._


<hr>

# Agent Guide — Infrastructure — Cloud to Code

> The path from a git push to production: containers, CI/CD, Infrastructure as Code, environments, and observability.
> Category: Architecture & Systems · Source: https://aliarsalan177.github.io/guides/infrastructure/

## Purpose

A field guide for **AI coding agents** (and engineers): the concepts to apply and the
mistakes to avoid when building with Infrastructure — Cloud to Code. Load this into your agent's context so it
keeps these concepts in mind and does **not** repeat these mistakes.

## Rules — Apply / Avoid

### 1. Infrastructure as Code
- ✅ **APPLY:** Define infra declaratively (Terraform/Pulumi/CloudFormation) in version control; review and apply via pipelines.
- ⛔ **AVOID:** Click-ops in the cloud console — undocumented, unrepeatable, and impossible to review or roll back.

### 2. Containerize for parity
- ✅ **APPLY:** Package apps in Docker (small, multi-stage images); orchestrate with Kubernetes/ECS for scaling and self-healing.
- ⛔ **AVOID:** 'Works on my machine' snowflake servers and fat images with secrets baked in.

### 3. Automate CI/CD
- ✅ **APPLY:** On push: lint, test, build, scan, then deploy through environments (dev → staging → prod) with rollbacks.
- ⛔ **AVOID:** Manual, ad-hoc deploys with no tests or gates — the classic source of production incidents.

### 4. Config & secrets by environment
- ✅ **APPLY:** Externalize config via env vars; store secrets in a manager (Vault, AWS Secrets Manager, SSM), not in code.
- ⛔ **AVOID:** Hard-coded credentials, committing .env, and per-environment code branches.

### 5. Observe and secure everything
- ✅ **APPLY:** Ship logs, metrics, traces and alerts; least-privilege IAM, network segmentation, TLS, and automated backups.
- ⛔ **AVOID:** Deploying blind with no monitoring, over-broad IAM roles, and untested backups.

## Cheat Reference — concepts to remember

- **Infrastructure as Code** — Define infra declaratively (Terraform/Pulumi/CloudFormation) in version control; review and apply via pipelines.
- **Containerize for parity** — Package apps in Docker (small, multi-stage images); orchestrate with Kubernetes/ECS for scaling and self-healing.
- **Automate CI/CD** — On push: lint, test, build, scan, then deploy through environments (dev → staging → prod) with rollbacks.
- **Config & secrets by environment** — Externalize config via env vars; store secrets in a manager (Vault, AWS Secrets Manager, SSM), not in code.
- **Observe and secure everything** — Ship logs, metrics, traces and alerts; least-privilege IAM, network segmentation, TLS, and automated backups.

## Full Cheat Sheet — every concept

### Cloud Foundations
- Providers: AWS/GCP/Azure; compute (EC2/VMs, containers, serverless/Lambda), storage (S3/blob), managed DBs.
- Networking: VPC, subnets, security groups, load balancers, DNS, CDN.
- IAM: least-privilege roles/policies; regions & availability zones for HA.

### Containers & Orchestration
- Docker: multi-stage builds, small base images, .dockerignore, no secrets in layers.
- Kubernetes/ECS: deployments, services, ingress, autoscaling, health probes, self-healing.
- Registries; image scanning; resource limits/requests.

### IaC & Config
- Terraform/Pulumi/CloudFormation in git; plan → review → apply via pipeline; remote state.
- 12-factor config via env vars; secrets in Vault/Secrets Manager/SSM.
- Immutable infrastructure; environment parity (dev/staging/prod).

### CI/CD
- Pipeline: install → lint → test → build → scan → deploy → verify.
- GitHub Actions/GitLab CI/Jenkins; artifact/image build; environment gates & approvals.
- Deploy strategies: rolling, blue-green, canary; automated rollback.

### Observability & Reliability
- Logs (ELK/Loki), metrics (Prometheus/Grafana/CloudWatch), traces (OpenTelemetry), alerting.
- SLIs/SLOs/SLAs; on-call/runbooks; incident response.
- Backups + tested restores, DR plan, TLS everywhere, WAF/DDoS protection.

## Interview Questions

#### Q1. What is Infrastructure as Code and why use it?
Defining infrastructure (servers, networks, databases) in declarative, version-controlled files (Terraform, CloudFormation) so environments are repeatable, reviewable, and auditable — instead of manual console changes.

#### Q2. Walk through a CI/CD pipeline.
On a push/PR: install deps, lint, run tests, build artifacts (e.g. a Docker image), scan for vulnerabilities, then deploy to staging and (after approval/gates) production — with automated rollback on failure.

#### Q3. Containers vs virtual machines?
VMs virtualize hardware with a full guest OS (heavier, stronger isolation); containers share the host kernel and package just the app + deps (lightweight, fast, portable). Containers give environment parity and density; orchestrators manage them at scale.

#### Q4. How do you manage secrets and config across environments?
Externalize config as environment variables per environment, and keep secrets in a dedicated manager (Vault, AWS Secrets Manager/SSM, sealed secrets) injected at runtime — never committed to the repo or baked into images.

#### Q5. Blue-green vs canary deployments?
Blue-green keeps two environments and switches traffic all at once (instant rollback). Canary shifts a small percentage of traffic to the new version, watches metrics, then ramps up — limiting blast radius of a bad release.

---

_Curated by Ali Arsalan · https://aliarsalan177.github.io · Generated from the Engineering Guides._


<hr>

# Agent Guide — Power BI

> Microsoft's BI platform: connect, model, and visualize data. Master Power Query (ETL), a star-schema model, and DAX.
> Category: AI & Data · Source: https://aliarsalan177.github.io/guides/power-bi/

## Purpose

A field guide for **AI coding agents** (and engineers): the concepts to apply and the
mistakes to avoid when building with Power BI. Load this into your agent's context so it
keeps these concepts in mind and does **not** repeat these mistakes.

## Rules — Apply / Avoid

### 1. Shape data in Power Query first
- ✅ **APPLY:** Do cleaning/typing/shaping in Power Query (M) — the ETL layer — before it hits the model.
- ⛔ **AVOID:** Loading raw, wide, messy tables and patching them with fragile DAX later.

### 2. Model as a star schema
- ✅ **APPLY:** Build fact tables surrounded by dimension tables with single-direction 1-to-many relationships.
- ⛔ **AVOID:** Flat one-big-table models and many-to-many/bidirectional relationships that break filter logic and performance.

### 3. Measures over calculated columns
- ✅ **APPLY:** Use DAX measures (computed at query time, respect filter context) for aggregations; understand row vs filter context.
- ⛔ **AVOID:** Calculated columns for aggregations — they bloat the model and don't respond to slicers like measures do.

### 4. Secure with Row-Level Security
- ✅ **APPLY:** Define RLS roles/filters so users only see their data; test with 'View as role'.
- ⛔ **AVOID:** Relying on hidden visuals/pages for security — the underlying data is still accessible.

### 5. Choose the right refresh/storage mode
- ✅ **APPLY:** Import for speed, DirectQuery for real-time/large sources, Incremental refresh for big historical tables.
- ⛔ **AVOID:** Full refresh of massive datasets every time, or DirectQuery on slow sources for interactive dashboards.

## Cheat Reference — concepts to remember

- **Shape data in Power Query first** — Do cleaning/typing/shaping in Power Query (M) — the ETL layer — before it hits the model.
- **Model as a star schema** — Build fact tables surrounded by dimension tables with single-direction 1-to-many relationships.
- **Measures over calculated columns** — Use DAX measures (computed at query time, respect filter context) for aggregations; understand row vs filter context.
- **Secure with Row-Level Security** — Define RLS roles/filters so users only see their data; test with 'View as role'.
- **Choose the right refresh/storage mode** — Import for speed, DirectQuery for real-time/large sources, Incremental refresh for big historical tables.

## Full Cheat Sheet — every concept

### Components
- Power BI Desktop (author reports), Service (cloud share/collaborate, workspaces), Mobile, Embedded, Report Server (on-prem).
- Views: Report, Data, Model (relationships).

### Power Query (ETL / M)
- Connect to files, databases, cloud & online services; 100s of connectors.
- Transform: filter, merge (JOIN), append (UNION), pivot/unpivot, split, typed columns.
- Reference queries, custom M functions; applied-steps are reproducible.

### Data Modeling
- Star schema: fact + dimension tables; relationships with cardinality & single cross-filter direction.
- Hierarchies, calculated tables, date/calendar table for time intelligence.

### DAX
- Measures (query-time) vs calculated columns (row-time); aggregations SUM/AVERAGE/COUNT.
- CALCULATE (modify filter context), FILTER, ALL, RELATED; iterators SUMX/AVERAGEX.
- Time intelligence: TOTALYTD, SAMEPERIODLASTYEAR, DATEADD.

### Visuals & Sharing
- Charts, tables, cards, maps, slicers; custom visuals from marketplace; drill-through & bookmarks.
- Reports vs dashboards; publish to Service; apps & subscriptions.

### Security & Refresh
- Row-Level Security (roles + DAX filters); on-premises data gateway.
- Refresh: Import / DirectQuery / Incremental; schedule refreshes.

## Interview Questions

#### Q1. Power Query vs DAX — what's each for?
Power Query (M language) is the ETL layer: connect, clean, transform, and shape data before load. DAX (Data Analysis Expressions) computes analytics over the loaded model — measures, calculated columns, and KPIs at query time.

#### Q2. Calculated column vs measure?
A calculated column is computed per row at refresh and stored in the model (row context). A measure is computed at query time and respects the current filter context (slicers, rows/columns) — use measures for aggregations.

#### Q3. What is a star schema and why use it?
Central fact tables (events/metrics) linked to dimension tables (attributes) via one-to-many relationships. It simplifies DAX, keeps filters flowing predictably, and performs far better than a flat table.

#### Q4. Import vs DirectQuery?
Import loads data into Power BI's in-memory engine (fast, but needs refresh). DirectQuery queries the source live (real-time, handles huge data, but slower and source-dependent). Composite models mix both.

#### Q5. What is row context vs filter context in DAX?
Row context is the current row (calculated columns, iterators like SUMX). Filter context is the set of filters applied (slicers, visuals). CALCULATE modifies filter context — the key to advanced DAX.

---

_Curated by Ali Arsalan · https://aliarsalan177.github.io · Generated from the Engineering Guides._


<hr>

# Agent Guide — TensorFlow & Keras

> Google's deep-learning framework. Build and train neural networks with the high-level Keras API, tf.data pipelines, and GPUs.
> Category: AI & Data · Source: https://aliarsalan177.github.io/guides/tensorflow/

## Purpose

A field guide for **AI coding agents** (and engineers): the concepts to apply and the
mistakes to avoid when building with TensorFlow & Keras. Load this into your agent's context so it
keeps these concepts in mind and does **not** repeat these mistakes.

## Rules — Apply / Avoid

### 1. Start with the Keras high-level API
- ✅ **APPLY:** Build models with Sequential/Functional Keras APIs; compile with optimizer + loss + metrics, then fit/evaluate/predict.
- ⛔ **AVOID:** Hand-rolling training loops and low-level ops before you need them — Keras covers most cases cleanly.

### 2. Feed data with tf.data pipelines
- ✅ **APPLY:** Use tf.data.Dataset with batch/shuffle/prefetch/map to stream data efficiently to the GPU.
- ⛔ **AVOID:** Loading the whole dataset into memory or feeding NumPy arrays for large data — it stalls training.

### 3. Guard against overfitting
- ✅ **APPLY:** Split train/validation/test, watch the validation curve, and use dropout, regularization, and early stopping.
- ⛔ **AVOID:** Judging a model on training accuracy alone — it memorizes and fails to generalize.

### 4. Match output layer & loss to the task
- ✅ **APPLY:** Softmax + categorical cross-entropy for multi-class; sigmoid + binary cross-entropy for binary; linear + MSE for regression.
- ⛔ **AVOID:** Mismatched activation/loss (e.g. softmax with MSE) — training won't converge properly.

### 5. Normalize inputs & use callbacks
- ✅ **APPLY:** Scale/normalize features; use callbacks (ModelCheckpoint, EarlyStopping, TensorBoard) to monitor and save.
- ⛔ **AVOID:** Training on unscaled features and no checkpoints — unstable training and lost best weights.

## Cheat Reference — concepts to remember

- **Start with the Keras high-level API** — Build models with Sequential/Functional Keras APIs; compile with optimizer + loss + metrics, then fit/evaluate/predict.
- **Feed data with tf.data pipelines** — Use tf.data.Dataset with batch/shuffle/prefetch/map to stream data efficiently to the GPU.
- **Guard against overfitting** — Split train/validation/test, watch the validation curve, and use dropout, regularization, and early stopping.
- **Match output layer & loss to the task** — Softmax + categorical cross-entropy for multi-class; sigmoid + binary cross-entropy for binary; linear + MSE for regression.
- **Normalize inputs & use callbacks** — Scale/normalize features; use callbacks (ModelCheckpoint, EarlyStopping, TensorBoard) to monitor and save.

## Full Cheat Sheet — every concept

### Core
- Tensors (n-dim arrays); eager execution by default; runs on CPU/GPU/TPU.
- TF 2.x centers on Keras; tf.function compiles Python to a graph for speed.

### Building Models (Keras)
- Sequential([...]) linear stack; Functional API for graphs; subclass Model for full control.
- Layers: Dense, Conv2D, MaxPooling2D, LSTM/GRU, Embedding, Dropout, BatchNormalization, Flatten.

### Training
- model.compile(optimizer, loss, metrics) → fit(x, y, epochs, batch_size, validation_data) → evaluate → predict.
- Optimizers: Adam, SGD, RMSprop; losses: cross-entropy (class), MSE/MAE (regression).
- epochs (passes), batch_size, learning rate; backprop + gradient descent.

### Data & Regularization
- tf.data.Dataset: from_tensor_slices, map, shuffle, batch, prefetch.
- Normalize inputs; augment images; train/val/test split.
- Overfitting: dropout, L1/L2, early stopping.

### Tooling
- Callbacks: ModelCheckpoint, EarlyStopping, TensorBoard, ReduceLROnPlateau.
- Save/load: model.save() / load_model() (SavedModel/Keras format).
- Transfer learning via tf.keras.applications; deploy with TF Serving / TFLite / TF.js.

## Interview Questions

#### Q1. What is a tensor?
A multi-dimensional array (scalars=0D, vectors=1D, matrices=2D, and higher) — the core data structure that flows through a TensorFlow computation graph, typically on CPU/GPU/TPU.

#### Q2. Sequential vs Functional Keras API?
Sequential stacks layers linearly (simple models). The Functional API connects layers as a graph, supporting multiple inputs/outputs, shared layers, and branches — needed for non-linear architectures.

#### Q3. How do you prevent overfitting?
Use a validation set to detect it, then apply dropout, L1/L2 regularization, early stopping, data augmentation, and more/cleaner data — so the model generalizes instead of memorizing.

#### Q4. What do optimizer, loss, and metrics do in compile()?
The loss quantifies error to minimize (e.g. cross-entropy), the optimizer updates weights via backprop (e.g. Adam/SGD), and metrics (e.g. accuracy) are tracked for human evaluation but not optimized directly.

#### Q5. What is transfer learning?
Reusing a pretrained model (e.g. on ImageNet) as a feature extractor and fine-tuning it on your smaller dataset — faster training and better results than training from scratch.

---

_Curated by Ali Arsalan · https://aliarsalan177.github.io · Generated from the Engineering Guides._


<hr>

# Agent Guide — Unity (Game Dev)

> Cross-platform game engine driven by C#. Think in GameObjects, Components, and the frame lifecycle — and keep the update loop cheap.
> Category: Frontend · Source: https://aliarsalan177.github.io/guides/unity/

## Purpose

A field guide for **AI coding agents** (and engineers): the concepts to apply and the
mistakes to avoid when building with Unity (Game Dev). Load this into your agent's context so it
keeps these concepts in mind and does **not** repeat these mistakes.

## Rules — Apply / Avoid

### 1. Compose with GameObjects + Components
- ✅ **APPLY:** Build behavior by attaching Components (scripts, colliders, renderers) to GameObjects — composition over inheritance.
- ⛔ **AVOID:** Giant monolithic MonoBehaviours that do everything — hard to reuse, test, and reason about.

### 2. Use the right lifecycle method
- ✅ **APPLY:** Awake/Start for setup, Update for per-frame logic, FixedUpdate for physics; cache references in Awake.
- ⛔ **AVOID:** Physics in Update or heavy per-frame allocation — it causes frame-rate-dependent bugs and GC spikes.

### 3. Keep Update() cheap
- ✅ **APPLY:** Cache GetComponent/Find results, pool objects, and avoid allocations in hot loops.
- ⛔ **AVOID:** Calling GetComponent/Find or instantiating/destroying every frame — it tanks performance and triggers GC.

### 4. Frame-rate-independent movement
- ✅ **APPLY:** Multiply movement/timers by Time.deltaTime so behavior is consistent across frame rates.
- ⛔ **AVOID:** Moving by a fixed amount per frame — speed then depends on the player's FPS.

### 5. Pool instead of Instantiate/Destroy
- ✅ **APPLY:** Reuse objects from an object pool for bullets/enemies/particles.
- ⛔ **AVOID:** Constant Instantiate()/Destroy() at runtime — it fragments memory and stutters via garbage collection.

## Cheat Reference — concepts to remember

- **Compose with GameObjects + Components** — Build behavior by attaching Components (scripts, colliders, renderers) to GameObjects — composition over inheritance.
- **Use the right lifecycle method** — Awake/Start for setup, Update for per-frame logic, FixedUpdate for physics; cache references in Awake.
- **Keep Update() cheap** — Cache GetComponent/Find results, pool objects, and avoid allocations in hot loops.
- **Frame-rate-independent movement** — Multiply movement/timers by Time.deltaTime so behavior is consistent across frame rates.
- **Pool instead of Instantiate/Destroy** — Reuse objects from an object pool for bullets/enemies/particles.

## Full Cheat Sheet — every concept

### Core Concepts
- GameObject + Components; Transform (position/rotation/scale); Scenes; Prefabs (reusable templates).
- Scripting in C# via MonoBehaviour; ScriptableObjects for data assets.

### Lifecycle
- Awake → OnEnable → Start (setup); Update (per frame); FixedUpdate (physics); LateUpdate (post).
- OnCollisionEnter/OnTriggerEnter for collisions; OnDestroy for cleanup.

### Physics & Input
- Rigidbody + Colliders; apply forces in FixedUpdate; isTrigger for overlap detection.
- Input System (or legacy Input); raycasting; layers & tags.

### Performance
- Cache GetComponent/references; object pooling; minimize allocations & GC.
- Time.deltaTime for frame independence; batch draw calls; use the Profiler.

### Systems & Build
- Coroutines (yield) for timed sequences; Animator/animation; UI (Canvas).
- Render pipelines (URP/HDRP); asset bundles/Addressables; build to PC/mobile/console/WebGL.

## Interview Questions

#### Q1. What are GameObjects and Components?
A GameObject is a container in the scene; Components (Transform, Rigidbody, Collider, custom MonoBehaviour scripts) attach to it to give behavior and data. Unity favors composition — you build entities by combining components.

#### Q2. Update vs FixedUpdate vs LateUpdate?
Update runs once per frame (variable timestep — gameplay/input). FixedUpdate runs at a fixed timestep (physics/Rigidbody). LateUpdate runs after all Updates (camera follow). Physics goes in FixedUpdate.

#### Q3. Why multiply by Time.deltaTime?
deltaTime is the seconds since the last frame; multiplying movement/timers by it makes behavior frame-rate independent, so speed is consistent whether running at 30 or 144 FPS.

#### Q4. How do you optimize a Unity game?
Cache component lookups, pool objects instead of Instantiate/Destroy, minimize per-frame allocations (avoid GC), batch draw calls, use the Profiler, and offload heavy work (jobs/coroutines).

#### Q5. What are Prefabs and ScriptableObjects?
Prefabs are reusable, instantiable GameObject templates. ScriptableObjects are data containers stored as assets — great for config/shared data without attaching to a scene object, reducing duplication.

---

_Curated by Ali Arsalan · https://aliarsalan177.github.io · Generated from the Engineering Guides._


<hr>

