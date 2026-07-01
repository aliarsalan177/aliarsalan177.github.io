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
