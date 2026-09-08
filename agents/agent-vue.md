# Agent Guide — Vue 3

> Progressive framework with a reactive core and the Composition API. Think in refs and computed state, keep props one-way, and let the template do the work.
> Category: Frontend · Source: https://ali-arsalan.com/guides/vue/

## Purpose

A field guide for **AI coding agents** (and engineers): the concepts to apply and the
mistakes to avoid when building with Vue 3. Load this into your agent's context so it
keeps these concepts in mind and does **not** repeat these mistakes.

## Rules — Apply / Avoid

### 1. Prefer the Composition API + <script setup>
- ✅ **APPLY:** Use <script setup> with ref/reactive/computed for concise, typed, reusable logic (composables).
- ⛔ **AVOID:** Forgetting to return values from setup() (they won't reach the template), or mixing Options + Composition randomly in one component.

### 2. ref vs reactive — and don't lose reactivity
- ✅ **APPLY:** Use ref() for primitives (access .value in JS; auto-unwrapped in templates); reactive() for objects. Use toRefs() when destructuring a reactive object.
- ⛔ **AVOID:** Destructuring a reactive() object directly (const { x } = state) — it breaks reactivity; and forgetting .value in script.

### 3. Keyed v-for, and don't combine with v-if
- ✅ **APPLY:** Give v-for a stable :key from your data; filter with a computed before rendering.
- ⛔ **AVOID:** v-if and v-for on the same element (ambiguous precedence), and index keys for reorderable lists.

### 4. Props down, events up
- ✅ **APPLY:** Declare inputs with defineProps and outputs with defineEmits; emit to notify the parent; use v-model for two-way binding.
- ⛔ **AVOID:** Mutating a prop inside the child — props are one-way; copy to local state or emit an update.

### 5. computed for derived state, watch for side effects
- ✅ **APPLY:** Use computed (cached) for values derived from state; use watch/watchEffect for side effects (fetch, DOM).
- ⛔ **AVOID:** Side effects inside computed, or reaching for watch when a computed would do (simpler and cached).

## Cheat Reference — concepts to remember

- **Prefer the Composition API + <script setup>** — Use <script setup> with ref/reactive/computed for concise, typed, reusable logic (composables).
- **ref vs reactive — and don't lose reactivity** — Use ref() for primitives (access .value in JS; auto-unwrapped in templates); reactive() for objects. Use toRefs() when destructuring a reactive object.
- **Keyed v-for, and don't combine with v-if** — Give v-for a stable :key from your data; filter with a computed before rendering.
- **Props down, events up** — Declare inputs with defineProps and outputs with defineEmits; emit to notify the parent; use v-model for two-way binding.
- **computed for derived state, watch for side effects** — Use computed (cached) for values derived from state; use watch/watchEffect for side effects (fetch, DOM).

## Full Cheat Sheet — every concept

### Template Syntax & Directives
- Interpolation {{ msg }} (expressions, not statements); v-text, v-html (raw HTML — beware XSS).
- v-if / v-else-if / v-else (DOM) vs v-show (CSS display); v-for with :key; v-once (render once).
- v-bind (:attr) reactive attributes; v-on (@event) listeners; v-model two-way binding.

### Events & Binding
- Event modifiers: .stop, .prevent, .once, .self, .capture; key modifiers (@keyup.enter).
- v-model modifiers: .lazy, .trim, .number; class/style binding with objects (:class="{ error: hasError }").

### Composition API
- <script setup> — everything is exposed to the template automatically.
- ref(v) → .value; reactive(obj); toRefs / toRef; computed(() => …); readonly.
- watch(src, cb, { immediate, deep }); watchEffect(() => …) runs immediately + on dep change.
- Composables: extract reusable stateful logic into useXxx() functions.

### Lifecycle Hooks
- onBeforeMount → onMounted (DOM ready) → onBeforeUpdate → onUpdated → onBeforeUnmount → onUnmounted.
- (Options API beforeCreate/created → use setup() instead.) Template refs: ref in template + a matching ref() in script.

### Components
- Props: defineProps (typed / with defaults); Emits: defineEmits; two-way via v-model.
- provide() / inject() for ancestor→descendant; <component :is> dynamic components; <KeepAlive> caches.

### Slots
- Default slot (<slot/>), named slots (<slot name="top"/> + <template #top>), scoped slots (child exposes data via v-bind, parent reads via slot props).

### Ecosystem
- Build: Vite (npm create vue@latest). Router: Vue Router (SPA routing).
- State: Pinia (the modern store; Vuex is legacy). Meta-framework: Nuxt (SSR/SSG).

## Interview Questions

#### Q1. Options API vs Composition API?
Options API organizes a component by option buckets (data, methods, computed, watch). The Composition API (setup / <script setup>) groups logic by concern using ref/reactive/computed and enables reusable composables — better for large components and TypeScript.

#### Q2. ref vs reactive?
ref() wraps any value (including primitives) and is accessed via .value in script (auto-unwrapped in templates); reactive() makes an object deeply reactive but can't hold primitives and loses reactivity if destructured (use toRefs). Many teams default to ref.

#### Q3. computed vs watch vs watchEffect?
computed returns a cached, derived value that updates when its dependencies change. watch observes specific sources and runs a callback (with old/new values) lazily. watchEffect runs immediately and re-runs whenever any reactive dependency it uses changes.

#### Q4. How does Vue 3 reactivity work?
It uses ES6 Proxies to track reads (dependency collection) and intercept writes (trigger updates), so it reactively re-renders only what depends on changed state — more capable than Vue 2's Object.defineProperty (which couldn't detect added properties).

#### Q5. v-if vs v-show?
v-if adds/removes the element from the DOM (cheaper when rarely shown, higher toggle cost); v-show only toggles the CSS display (element stays in the DOM, cheap to toggle). Use v-show for frequent toggles.

#### Q6. How do components communicate?
Parent → child via props; child → parent via emitted events (defineEmits/$emit); two-way with v-model; distant ancestors via provide/inject; and cross-app via a store (Pinia). Slots pass template content down.

---

_Curated by Ali Arsalan · https://ali-arsalan.com · Generated from the Engineering Guides._
