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
