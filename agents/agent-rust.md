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

### Collections & Strings
- Vec<T> (growable array), HashMap<K,V>, HashSet, arrays [T; N], slices &[T].
- String (owned, growable) vs &str (borrowed string slice).
- Constants (const), shadowing, tuples, destructuring assignment.

### Closures & Iterators
- Closures capture environment: Fn (borrow), FnMut (mutate), FnOnce (consume).
- Iterators are lazy: .iter()/.into_iter(); adapters .map/.filter/.collect/.fold; for loops call into_iter().

### Smart Pointers
- Box<T> heap allocation / recursive types; Rc<T> shared ownership (single-thread).
- RefCell<T> interior mutability (runtime borrow checks); combine Rc<RefCell<T>>.
- Arc<T> is the thread-safe Rc; Deref/Drop traits.

### Concurrency
- thread::spawn + join; move closures to transfer ownership into threads.
- Share with Arc<Mutex<T>> (lock to access); message-pass with channels (mpsc).
- Fearless concurrency: the borrow checker prevents data races at compile time; async/await for I/O.

### Traits, Generics & Macros
- Trait objects (dyn Trait) vs generics with bounds; associated types; Default/Copy/Clone/From/Into.
- From/Into & TryFrom/TryInto for conversions; pattern-matching enums exhaustively.
- Declarative macros (macro_rules!) with fragment specifiers; doctests & /// documentation comments.

## Interview Questions

#### Q1. What is ownership in Rust?
Each value has a single owner; when the owner goes out of scope the value is dropped. Assigning or passing a non-Copy value moves ownership, invalidating the original binding — this gives memory safety without a GC.

#### Q2. What are the borrowing rules?
At any time you can have either any number of immutable references (&T) or exactly one mutable reference (&mut T), but not both. This prevents data races and aliasing bugs at compile time.

#### Q3. Option and Result vs null/exceptions?
Option<T> encodes presence/absence and Result<T,E> encodes success/failure in the type system, forcing you to handle both cases. There's no null and no exceptions; the `?` operator ergonomically propagates errors.

#### Q4. What are lifetimes?
Annotations that tell the compiler how long references must remain valid so it can guarantee no reference outlives the data it points to, preventing dangling references.

#### Q5. Box vs Rc vs RefCell?
Box<T> is single-owner heap allocation (recursive types). Rc<T> allows multiple owners (reference-counted, single-threaded). RefCell<T> moves borrow checking to runtime for interior mutability; Rc<RefCell<T>> combines shared ownership + mutation. Arc<Mutex<T>> is the thread-safe equivalent.

#### Q6. How does Rust achieve 'fearless concurrency'?
The ownership and borrowing rules are enforced across threads too, so data races are caught at compile time. You share state with Arc<Mutex<T>> or pass messages over channels, and move closures transfer ownership into threads.

#### Q7. Fn vs FnMut vs FnOnce?
Traits for closures by how they capture: Fn borrows immutably, FnMut borrows mutably, FnOnce takes ownership and can be called once. The compiler infers the least restrictive that fits.

---

_Curated by Ali Arsalan · https://aliarsalan177.github.io · Generated from the Engineering Guides._
