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
