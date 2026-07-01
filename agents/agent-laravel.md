# Agent Guide — Laravel

> Batteries-included PHP framework: Eloquent, Blade, queues, and elegant APIs. Lean on the conventions and its built-in security (aligned with OWASP).
> Category: Backend & Infra · Source: https://aliarsalan177.github.io/guides/laravel/

## Purpose

A field guide for **AI coding agents** (and engineers): the concepts to apply and the
mistakes to avoid when building with Laravel. Load this into your agent's context so it
keeps these concepts in mind and does **not** repeat these mistakes.

## Rules — Apply / Avoid

### 1. Guard against mass assignment
- ✅ **APPLY:** Fill models from $request->validated() or ->only([...]); set $fillable explicitly.
- ⛔ **AVOID:** Model::create($request->all()) with empty $guarded / unguarded models — attackers can set any column.

### 2. Query safely with Eloquent
- ✅ **APPLY:** Use Eloquent/Query Builder (parameterized by default); for raw SQL, bind values: whereRaw('x = ?', [$v]).
- ⛔ **AVOID:** Interpolating input into raw queries or letting users choose column names without a whitelist.

### 3. Escape output & keep CSRF on
- ✅ **APPLY:** Use Blade {{ }} (auto-escapes) and @csrf on POST forms; keep VerifyCsrfToken on web routes.
- ⛔ **AVOID:** {!! !!} on untrusted data (XSS) and disabling CSRF except for stateless APIs/webhooks.

### 4. Authorize with policies/gates
- ✅ **APPLY:** Centralize permissions in Policies/Gates; use Sanctum (SPA/tokens) or Passport (OAuth2) for APIs.
- ⛔ **AVOID:** Ad-hoc permission checks scattered in controllers, or trusting the client.

### 5. Harden config & production
- ✅ **APPLY:** APP_DEBUG=false in prod, run key:generate, encrypt cookies, set secure/httpOnly/sameSite, throttle routes, add security headers.
- ⛔ **AVOID:** Debug mode on in production, committing .env, and unbounded (unthrottled) endpoints.

### 6. Offload slow work to queues
- ✅ **APPLY:** Dispatch jobs to queues (mail, images, webhooks); use events/listeners and the scheduler.
- ⛔ **AVOID:** Doing slow work inside the request/response cycle and blocking users.

## Cheat Reference — concepts to remember

- **Guard against mass assignment** — Fill models from $request->validated() or ->only([...]); set $fillable explicitly.
- **Query safely with Eloquent** — Use Eloquent/Query Builder (parameterized by default); for raw SQL, bind values: whereRaw('x = ?', [$v]).
- **Escape output & keep CSRF on** — Use Blade {{ }} (auto-escapes) and @csrf on POST forms; keep VerifyCsrfToken on web routes.
- **Authorize with policies/gates** — Centralize permissions in Policies/Gates; use Sanctum (SPA/tokens) or Passport (OAuth2) for APIs.
- **Harden config & production** — APP_DEBUG=false in prod, run key:generate, encrypt cookies, set secure/httpOnly/sameSite, throttle routes, add security headers.
- **Offload slow work to queues** — Dispatch jobs to queues (mail, images, webhooks); use events/listeners and the scheduler.

## Full Cheat Sheet — every concept

### MVC & Routing
- Routes (web.php/api.php) → Controllers → Views (Blade) / JSON resources.
- Route model binding; middleware groups (web, api); resource controllers.

### Eloquent ORM
- Models, migrations, factories, seeders; relationships (hasMany, belongsTo, morphTo).
- $fillable/$guarded for mass assignment; eager load with with() to avoid N+1.
- Query Builder + Eloquent are parameterized; bind raw queries.

### Requests & Validation
- Form Requests / $request->validate(); use validated()/only(), not all().
- Blade {{ }} escapes; {!! !!} only for trusted HTML.

### Auth & Authorization
- Starter kits: Breeze, Fortify, Jetstream. APIs: Sanctum / Passport.
- Gates & Policies for authorization; guards (session/token) + providers (eloquent/database).

### Security (OWASP)
- APP_DEBUG=false; key:generate; EncryptCookies; secure/httpOnly/sameSite cookies.
- CSRF (@csrf, VerifyCsrfToken); throttle middleware (rate limiting); validate uploads + basename().
- Security headers (CSP, HSTS, X-Frame-Options); never unserialize/eval/extract untrusted data.

### Async & Ecosystem
- Queues & Jobs, Events/Listeners, Task Scheduler; Cache, Broadcasting, Notifications.
- Artisan CLI; Tinker; Telescope/Horizon; Pest/PHPUnit tests.

## Interview Questions

#### Q1. How does Laravel prevent mass-assignment vulnerabilities?
Models restrict which attributes can be bulk-assigned via $fillable (allowlist) or $guarded (blocklist). Best practice is to fill from validated input ($request->validated()) rather than $request->all().

#### Q2. How does Blade protect against XSS?
{{ $var }} auto-escapes output with htmlspecialchars. Only {!! $var !!} outputs raw HTML — use it exclusively for trusted content, never untrusted user input.

#### Q3. Sanctum vs Passport?
Sanctum is lightweight token/SPA cookie auth for first-party apps and simple API tokens; Passport is a full OAuth2 server for third-party API access. Pick Sanctum unless you need OAuth2 flows.

#### Q4. What is Eloquent and how does it avoid SQL injection?
Eloquent is Laravel's ActiveRecord ORM; it and the Query Builder use PDO prepared statements with bound parameters by default, so ordinary queries are injection-safe. Raw queries must bind values explicitly.

#### Q5. Key production hardening steps in Laravel?
APP_DEBUG=false, generate APP_KEY, keep CSRF + EncryptCookies middleware, set secure cookie flags, apply throttle middleware, validate file uploads, and add security headers (CSP, HSTS, X-Frame-Options).

---

_Curated by Ali Arsalan · https://aliarsalan177.github.io · Generated from the Engineering Guides._
