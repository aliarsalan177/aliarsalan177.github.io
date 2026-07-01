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
