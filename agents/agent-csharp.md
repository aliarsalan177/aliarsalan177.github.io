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
- LINQ: declarative queries (Where/Select/OrderBy) or query syntax; deferred until enumerated (ToList/Count).
- Delegates, events, Func<>/Action<> + lambdas ((a,b)=>a+b) for callbacks/pub-sub.
- Extension methods add methods to existing types (need `using` of their namespace); nameof, attributes.

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
