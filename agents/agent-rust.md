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
