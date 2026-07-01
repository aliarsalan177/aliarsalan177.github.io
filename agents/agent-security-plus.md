# Agent Guide — Security+ (Fundamentals)

> The baseline security body of knowledge: risk, identity, cryptography and controls — the CompTIA Security+ core.
> Category: CS & Security · Source: https://aliarsalan177.github.io/guides/security-plus/

## Purpose

A field guide for **AI coding agents** (and engineers): the concepts to apply and the
mistakes to avoid when building with Security+ (Fundamentals). Load this into your agent's context so it
keeps these concepts in mind and does **not** repeat these mistakes.

## Rules — Apply / Avoid

### 1. Think in risk, not just threats
- ✅ **APPLY:** Assess risk = likelihood × impact, then treat it (mitigate, transfer, accept, avoid) and prioritize accordingly.
- ⛔ **AVOID:** Chasing every theoretical threat equally instead of protecting the highest-value, highest-likelihood assets.

### 2. Identity & access management
- ✅ **APPLY:** Enforce MFA, least privilege, and role-based access; separate duties for sensitive actions.
- ⛔ **AVOID:** Shared accounts and standing over-privileged access that make breaches total and untraceable.

### 3. Use the right crypto for the job
- ✅ **APPLY:** Hash with salted KDFs, encrypt in transit (TLS) and at rest, and manage keys properly.
- ⛔ **AVOID:** Home-grown crypto, weak/legacy algorithms, and hard-coded or unrotated keys.

### 4. Defense in depth + prepare to respond
- ✅ **APPLY:** Layer controls (network, host, app, data) and have an incident response plan: prepare, detect, contain, eradicate, recover.
- ⛔ **AVOID:** A single perimeter with no monitoring or IR plan — you won't detect or survive a breach.

## Cheat Reference — concepts to remember

- **Think in risk, not just threats** — Assess risk = likelihood × impact, then treat it (mitigate, transfer, accept, avoid) and prioritize accordingly.
- **Identity & access management** — Enforce MFA, least privilege, and role-based access; separate duties for sensitive actions.
- **Use the right crypto for the job** — Hash with salted KDFs, encrypt in transit (TLS) and at rest, and manage keys properly.
- **Defense in depth + prepare to respond** — Layer controls (network, host, app, data) and have an incident response plan: prepare, detect, contain, eradicate, recover.

## Full Cheat Sheet — every concept

### Risk & Governance
- CIA triad; risk = likelihood × impact; treat by mitigate/transfer/accept/avoid.
- Policies, standards, compliance (GDPR/PCI/HIPAA), security awareness.

### Identity & Access
- AAA (authentication, authorization, accounting); MFA; RBAC; least privilege; separation of duties.

### Cryptography
- Symmetric (AES) vs asymmetric (RSA/ECC); hashing (SHA-2) + salted KDFs for passwords.
- PKI, certificates, TLS; proper key management/rotation.

### Network & Threats
- Firewalls, VPN, IDS/IPS, segmentation, zero trust.
- Attacks: phishing, malware, MITM, DoS/DDoS, injection; defense in depth.

### Operations
- Vulnerability assessment & pentesting; patch management.
- Incident response lifecycle; logging/monitoring; backups & disaster recovery.

### Key Acronyms
- CIA, AAA, MFA, SSO, RBAC, IAM, PKI, CA, TLS/SSL, VPN, DMZ.
- IDS/IPS, SIEM, SOC, DLP, WAF, EDR, NAC, ACL.
- Threats: DDoS, MITM, RCE, XSS, CSRF, APT, IOC, RAT.
- Recovery/metrics: RTO, RPO, MTTR, MTBF, BCP, DRP; CVE, CVSS.

## Interview Questions

#### Q1. What is the CIA triad?
Confidentiality, Integrity, and Availability — the three core goals of information security that guide control selection and trade-offs.

#### Q2. What is defense in depth?
Layering multiple, independent security controls (physical, network, host, application, data) so that if one fails, others still protect the asset.

#### Q3. Authentication vs authorization?
Authentication verifies who you are (passwords, MFA, biometrics); authorization determines what you're allowed to do (permissions, roles). Both are checked, server-side, on every request.

#### Q4. How do you handle a security incident?
Follow an IR lifecycle: preparation, identification/detection, containment, eradication, recovery, and lessons learned — to limit damage and prevent recurrence.

---

_Curated by Ali Arsalan · https://aliarsalan177.github.io · Generated from the Engineering Guides._
