# Agent Guide — Cybersecurity

> Protect confidentiality, integrity and availability. Assume breach, least privilege, and defense in depth.
> Category: CS & Security · Source: https://aliarsalan177.github.io/guides/cybersecurity/

## Purpose

A field guide for **AI coding agents** (and engineers): the concepts to apply and the
mistakes to avoid when building with Cybersecurity. Load this into your agent's context so it
keeps these concepts in mind and does **not** repeat these mistakes.

## Rules — Apply / Avoid

### 1. Never trust input
- ✅ **APPLY:** Validate and sanitize all input; use parameterized queries and output encoding.
- ⛔ **AVOID:** Concatenating user input into SQL or HTML — the root of SQL injection and XSS.

### 2. Least privilege & defense in depth
- ✅ **APPLY:** Grant the minimum access needed and layer controls (network, app, data) so one failure isn't fatal.
- ⛔ **AVOID:** Broad admin credentials and a single perimeter — one breach then owns everything.

### 3. Handle secrets and crypto correctly
- ✅ **APPLY:** Hash passwords with a slow salted KDF (bcrypt/argon2), encrypt in transit and at rest, use vetted libraries.
- ⛔ **AVOID:** Rolling your own crypto, storing plaintext/MD5 passwords, or committing secrets to the repo.

### 4. Authenticate then authorize
- ✅ **APPLY:** Verify identity (MFA) and separately check permissions on every request, server-side.
- ⛔ **AVOID:** Trusting client-side checks or a hidden UI for security — attackers call the API directly.

## Cheat Reference — concepts to remember

- **Never trust input** — Validate and sanitize all input; use parameterized queries and output encoding.
- **Least privilege & defense in depth** — Grant the minimum access needed and layer controls (network, app, data) so one failure isn't fatal.
- **Handle secrets and crypto correctly** — Hash passwords with a slow salted KDF (bcrypt/argon2), encrypt in transit and at rest, use vetted libraries.
- **Authenticate then authorize** — Verify identity (MFA) and separately check permissions on every request, server-side.

## Full Cheat Sheet — every concept

### Principles
- CIA triad: Confidentiality, Integrity, Availability.
- Least privilege, defense in depth, assume breach, separation of duties.

### Threats & Attacks
- Phishing/social engineering, malware, DDoS, MITM, data breach.
- Web: SQL injection & XSS (untrusted input treated as code), CSRF.

### Identity & Crypto
- Authentication (who) with MFA vs authorization (what) — check both server-side.
- Symmetric (shared key, bulk) vs asymmetric (key pair, exchange/signatures); TLS combines both.
- Hash passwords with salted bcrypt/argon2; encrypt in transit & at rest; never roll your own crypto.

### Defense & Response
- Firewalls, VPN, IDS/IPS, antivirus, WAF.
- Validate/sanitize input; parameterized queries; output encoding; CSP.
- Incident response: prepare → detect → contain → eradicate → recover → learn.

## Interview Questions

#### Q1. What is the CIA triad?
Confidentiality (only authorized access), Integrity (data isn't tampered with), and Availability (systems are up when needed) — the three goals that frame most security decisions.

#### Q2. How do you prevent SQL injection and XSS?
SQL injection: use parameterized queries/ORMs and validate input. XSS: encode/escape output for its context and use a Content-Security-Policy. Both stem from treating untrusted input as trusted code.

#### Q3. Symmetric vs asymmetric encryption?
Symmetric uses one shared secret key (fast, for bulk data); asymmetric uses a public/private key pair (for key exchange and signatures). TLS combines both: asymmetric to agree on a symmetric session key.

#### Q4. Why hash passwords with bcrypt/argon2 instead of SHA-256?
Password hashes should be deliberately slow and salted to resist brute-force and rainbow tables. Fast general-purpose hashes (SHA-256, MD5) are trivially crackable at scale.

---

_Curated by Ali Arsalan · https://aliarsalan177.github.io · Generated from the Engineering Guides._
