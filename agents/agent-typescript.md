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
