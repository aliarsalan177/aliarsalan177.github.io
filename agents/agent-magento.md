# Agent Guide — Magento 2

> Enterprise e-commerce on a modular, DI-driven PHP architecture. Extend the right way — plugins and DI, never core edits.
> Category: Backend & Infra · Source: https://aliarsalan177.github.io/guides/magento/

## Purpose

A field guide for **AI coding agents** (and engineers): the concepts to apply and the
mistakes to avoid when building with Magento 2. Load this into your agent's context so it
keeps these concepts in mind and does **not** repeat these mistakes.

## Rules — Apply / Avoid

### 1. Never edit core; extend via modules
- ✅ **APPLY:** Build custom modules; change behavior with plugins (interceptors), preferences, and events/observers.
- ⛔ **AVOID:** Editing vendor/magento core files — upgrades wipe them and it breaks the platform.

### 2. Use dependency injection, not ObjectManager
- ✅ **APPLY:** Inject dependencies via the constructor and di.xml; use factories/proxies for non-injectables.
- ⛔ **AVOID:** Calling ObjectManager::getInstance() directly in business code — it hides deps and is an anti-pattern.

### 3. Prefer plugins over rewrites
- ✅ **APPLY:** Use before/after/around plugins to modify public methods without replacing whole classes.
- ⛔ **AVOID:** Class preferences (rewrites) when a plugin would do — rewrites collide and are fragile across modules.

### 4. Respect service contracts & indexing/cache
- ✅ **APPLY:** Program against Api/ service-contract interfaces and repositories; keep indexers and cache in mind.
- ⛔ **AVOID:** Direct SQL/resource-model hacks, and forgetting to reindex / flush cache after data or config changes.

### 5. Optimize for performance
- ✅ **APPLY:** Use Varnish full-page cache, Redis (session/cache), OpenSearch, production mode, and flat/async indexing.
- ⛔ **AVOID:** Running developer mode in production and uncached, unindexed catalog queries at scale.

## Cheat Reference — concepts to remember

- **Never edit core; extend via modules** — Build custom modules; change behavior with plugins (interceptors), preferences, and events/observers.
- **Use dependency injection, not ObjectManager** — Inject dependencies via the constructor and di.xml; use factories/proxies for non-injectables.
- **Prefer plugins over rewrites** — Use before/after/around plugins to modify public methods without replacing whole classes.
- **Respect service contracts & indexing/cache** — Program against Api/ service-contract interfaces and repositories; keep indexers and cache in mind.
- **Optimize for performance** — Use Varnish full-page cache, Redis (session/cache), OpenSearch, production mode, and flat/async indexing.

## Full Cheat Sheet — every concept

### Module Structure
- app/code/Vendor/Module with registration.php + etc/module.xml.
- etc/di.xml (DI, plugins, preferences), events.xml, acl.xml, adminhtml/frontend areas.
- Model/ ResourceModel/ Api/ Block/ Controller/ view/ Setup or declarative schema (db_schema.xml).

### Dependency Injection
- Constructor injection; di.xml for preferences, plugins, virtual types.
- Factories for non-injectable (entity) objects; Proxies for lazy/expensive deps.
- Never use ObjectManager in business logic.

### Extension Points
- Plugins (before/after/around) on public methods; Observers on events; Preferences (rewrite, last resort).
- UI Components (grids/forms) via XML; layout XML + templates (.phtml) + ViewModels.

### Data & APIs
- Service contracts (Api/ interfaces) + repositories; data interfaces.
- REST & GraphQL; declarative schema (db_schema.xml) + data/schema patches.

### Performance & Ops
- Modes: default/developer/production; bin/magento setup:di:compile, setup:static-content:deploy.
- Varnish FPC, Redis cache/session, OpenSearch; indexers (bin/magento indexer:reindex); cache:flush.

## Interview Questions

#### Q1. How do you customize Magento 2 without touching core?
Create a custom module and change behavior with plugins (interceptors), event observers, preferences (class rewrites as a last resort), and configuration via di.xml/XML — so upgrades don't overwrite your changes.

#### Q2. Plugin (interceptor) vs preference?
A plugin wraps a public method (before/after/around) to modify inputs, outputs, or flow without replacing the class, so multiple modules can coexist. A preference replaces the whole class implementation and can conflict — use it only when a plugin can't.

#### Q3. Why avoid ObjectManager directly?
It hides dependencies, breaks testability, and bypasses Magento's DI. Inject dependencies through the constructor (resolved via di.xml); use factories for objects you must create at runtime.

#### Q4. What are service contracts?
Api/ interfaces (data interfaces + service interfaces like repositories) that define a stable public API for a module, decoupling callers from implementation and enabling web APIs (REST/GraphQL).

#### Q5. Key Magento 2 performance levers?
Production mode, Varnish full-page cache, Redis for cache/sessions, OpenSearch/Elasticsearch, proper indexing (schedule/async), composer autoloader optimization, and a CDN for static/media.

---

_Curated by Ali Arsalan · https://aliarsalan177.github.io · Generated from the Engineering Guides._
