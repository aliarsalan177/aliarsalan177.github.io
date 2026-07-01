# Agent Guide — PHP (Modern / Vanilla)

> Modern PHP 8+ is typed, fast and Composer-driven. Write strict, secure, PSR-compliant code — not the legacy stuff of old.
> Category: Languages · Source: https://aliarsalan177.github.io/guides/php/

## Purpose

A field guide for **AI coding agents** (and engineers): the concepts to apply and the
mistakes to avoid when building with PHP (Modern / Vanilla). Load this into your agent's context so it
keeps these concepts in mind and does **not** repeat these mistakes.

## Rules — Apply / Avoid

### 1. Write typed, strict modern PHP
- ✅ **APPLY:** Add declare(strict_types=1), type-hint params/returns, use enums, readonly props, and named arguments (PHP 8+).
- ⛔ **AVOID:** Loose, untyped legacy style with `@` error suppression and implicit type juggling.

### 2. Never trust input — escape by context
- ✅ **APPLY:** Validate/filter input; use prepared statements (PDO/mysqli) for SQL; htmlspecialchars() on output.
- ⛔ **AVOID:** String-concatenated SQL and echoing raw user input — SQL injection and XSS.

### 3. Use Composer + PSR standards
- ✅ **APPLY:** Manage deps with Composer, autoload via PSR-4, follow PSR-12 style; don't reinvent libraries.
- ⛔ **AVOID:** Manual require chains and copy-pasted vendor code with no autoloading or dependency management.

### 4. Handle errors with exceptions
- ✅ **APPLY:** Throw/catch typed exceptions; log server-side; return safe messages to users.
- ⛔ **AVOID:** Suppressing errors with @, exposing stack traces, or leaving display_errors on in production.

### 5. Hash secrets and manage config
- ✅ **APPLY:** password_hash()/password_verify() (bcrypt/argon2); keep secrets in env, not code.
- ⛔ **AVOID:** md5/sha1 for passwords, hard-coded credentials, and committing .env.

## Cheat Reference — concepts to remember

- **Write typed, strict modern PHP** — Add declare(strict_types=1), type-hint params/returns, use enums, readonly props, and named arguments (PHP 8+).
- **Never trust input — escape by context** — Validate/filter input; use prepared statements (PDO/mysqli) for SQL; htmlspecialchars() on output.
- **Use Composer + PSR standards** — Manage deps with Composer, autoload via PSR-4, follow PSR-12 style; don't reinvent libraries.
- **Handle errors with exceptions** — Throw/catch typed exceptions; log server-side; return safe messages to users.
- **Hash secrets and manage config** — password_hash()/password_verify() (bcrypt/argon2); keep secrets in env, not code.

## Full Cheat Sheet — every concept

### Language Basics
- declare(strict_types=1); scalar types (int, float, string, bool), arrays, objects, null.
- Variables $x; arrays (indexed & associative); string interpolation "$x" and heredoc.
- == juggles types, === strict; the spaceship <=> for comparison.

### Modern PHP 8+
- Constructor property promotion, readonly properties, enums, union/intersection types.
- match (strict, returns a value), nullsafe ?->, named args, first-class callable syntax.
- Attributes (#[Route(...)]); the JIT compiler.

### OOP
- class/interface/trait/abstract; visibility public/protected/private; static; final.
- Namespaces + PSR-4 autoloading via Composer.
- Exceptions: try/catch/finally, custom exception classes.

### Security (critical)
- SQL: PDO prepared statements with bound params; whitelist dynamic identifiers.
- XSS: htmlspecialchars() on output; CSRF tokens on forms.
- Passwords: password_hash()/password_verify(); never unserialize()/eval()/extract() untrusted data.
- Config: env vars for secrets; APP debug off in prod; file perms tight.

### Tooling & Standards
- Composer (deps + autoload), PSR-4/PSR-12; PHPUnit/Pest for tests.
- Static analysis (PHPStan/Psalm), formatting (PHP-CS-Fixer).

## Interview Questions

#### Q1. == vs === in PHP?
== compares with type juggling (0 == 'a' was true before PHP 8); === compares value and type with no coercion. Prefer === to avoid surprising conversions (PHP 8 also fixed many juggling quirks).

#### Q2. How do you prevent SQL injection in PHP?
Use prepared statements with bound parameters via PDO or mysqli, never concatenate user input into SQL. Validate and whitelist anything that can't be bound (like column names).

#### Q3. What are PSR standards and Composer?
PSRs are PHP-FIG recommendations (PSR-4 autoloading, PSR-12 coding style, PSR-7 HTTP messages). Composer is the dependency manager that autoloads packages and your code per PSR-4.

#### Q4. What's new in modern PHP (8.x)?
Typed properties, union types, enums, readonly properties, named arguments, match expressions, nullsafe operator (?->), constructor property promotion, attributes, and the JIT compiler.

#### Q5. How should passwords be stored?
With password_hash() (bcrypt/argon2, salted and slow) and verified with password_verify() — never md5/sha1 or plaintext.

---

_Curated by Ali Arsalan · https://aliarsalan177.github.io · Generated from the Engineering Guides._
