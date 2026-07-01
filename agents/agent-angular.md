# Agent Guide — Angular

> A batteries-included framework: components, DI, RxJS, and (increasingly) signals. Lean on the structure, mind change detection.
> Category: Frontend · Source: https://aliarsalan177.github.io/guides/angular/

## Purpose

A field guide for **AI coding agents** (and engineers): the concepts to apply and the
mistakes to avoid when building with Angular. Load this into your agent's context so it
keeps these concepts in mind and does **not** repeat these mistakes.

## Rules — Apply / Avoid

### 1. Dependency Injection is the backbone
- ✅ **APPLY:** Put shared logic in @Injectable services and inject via the constructor; scope providers deliberately (root vs component).
- ⛔ **AVOID:** Instantiating services with `new` or duplicating provider registrations — you lose singletons and DI benefits.

### 2. Unsubscribe / use the async pipe
- ✅ **APPLY:** Prefer the `| async` pipe so Angular manages subscription lifecycle, or takeUntil/takeUntilDestroyed for manual ones.
- ⛔ **AVOID:** Subscribing in components without tearing down — long-lived subscriptions leak memory and fire after destroy.

### 3. Change detection has a cost
- ✅ **APPLY:** Use OnPush change detection with immutable inputs (or signals) so Angular checks only what actually changed.
- ⛔ **AVOID:** Doing heavy work or creating new object references in templates/getters — it runs on every CD cycle.

### 4. Reactive forms for anything non-trivial
- ✅ **APPLY:** Use reactive (typed) forms for validation, dynamic controls and testability.
- ⛔ **AVOID:** Forgetting to import FormsModule/ReactiveFormsModule, or mixing template- and reactive-driven on one control.

## Cheat Reference — concepts to remember

- **Dependency Injection is the backbone** — Put shared logic in @Injectable services and inject via the constructor; scope providers deliberately (root vs component).
- **Unsubscribe / use the async pipe** — Prefer the `| async` pipe so Angular manages subscription lifecycle, or takeUntil/takeUntilDestroyed for manual ones.
- **Change detection has a cost** — Use OnPush change detection with immutable inputs (or signals) so Angular checks only what actually changed.
- **Reactive forms for anything non-trivial** — Use reactive (typed) forms for validation, dynamic controls and testability.

## Full Cheat Sheet — every concept

### CLI & Project
- npm i -g @angular/cli; ng new my-app; ng serve (dev); ng build (prod).
- Generate: ng g c Name (component), ng g s Name (service), ng g d/p/m (directive/pipe/module).
- ng add @angular/material installs + configures Angular-optimized packages.

### Components & Modules
- Component = template + encapsulated CSS + TS class; generate with `ng g c Name`.
- Files: .component.html/.css/.ts/.spec.ts; decorated with @Component, registered to a module.
- @NgModule: declarations, imports, exports, providers, bootstrap (or standalone components).

### Data Binding
- Interpolation {{ expr }}, property [prop]="v", event (event)="handler()", two-way [(ngModel)]="x".
- ngModel needs FormsModule.

### Directives & Pipes
- Structural (*ngIf, *ngFor, *ngSwitch) add/remove DOM — don't forget the asterisk.
- Attribute (ngClass, ngStyle) change appearance/behavior.
- Pipes transform in templates: {{ value | date }} / uppercase / currency.

### Services & DI
- @Injectable services + constructor injection; scope providers (root vs component).
- Provide before injecting; root providers are singletons.

### Reactivity & Performance
- RxJS Observables for async; prefer the | async pipe (auto-unsubscribe) or takeUntilDestroyed.
- OnPush change detection with immutable inputs / signals to minimize checks.
- Full lifecycle: ngOnChanges → ngOnInit → ngDoCheck → ngAfterContentInit/Checked → ngAfterViewInit/Checked → ngOnDestroy.
- @Injectable providedIn: 'root' (app-wide singleton), module providers, or component providers.

## Interview Questions

#### Q1. What is dependency injection in Angular?
A system where Angular creates and supplies service instances to components/services that declare them in their constructor. Providers control scope and lifetime (root-level singletons vs per-component instances).

#### Q2. Structural vs attribute directives?
Structural directives (*ngIf, *ngFor, *ngSwitch) add/remove DOM. Attribute directives (ngClass, ngStyle, ngModel) change the appearance or behavior of an existing element.

#### Q3. What is OnPush change detection?
A strategy that only re-checks a component when its @Input references change, an event fires from it, or an observable it uses (via async pipe) emits — reducing needless checks. It pairs with immutable data or signals.

#### Q4. Template-driven vs reactive forms?
Template-driven forms live in the template with ngModel (simple cases); reactive forms define the model in TypeScript with FormGroup/FormControl, giving explicit, typed, testable control and dynamic validation.

---

_Curated by Ali Arsalan · https://aliarsalan177.github.io · Generated from the Engineering Guides._
