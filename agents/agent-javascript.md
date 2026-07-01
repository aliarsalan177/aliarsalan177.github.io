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
