# Agent Guide — Nmap (Network Scanning)

> The standard tool for host discovery and port scanning. Powerful for defense and recon — only ever with authorization.
> Category: CS & Security · Source: https://aliarsalan177.github.io/guides/nmap/

## Purpose

A field guide for **AI coding agents** (and engineers): the concepts to apply and the
mistakes to avoid when building with Nmap (Network Scanning). Load this into your agent's context so it
keeps these concepts in mind and does **not** repeat these mistakes.

## Rules — Apply / Avoid

### 1. Authorization first
- ✅ **APPLY:** Only scan networks you own or have explicit written permission to test.
- ⛔ **AVOID:** Scanning third-party or public hosts without consent — it's noisy, detectable, and often illegal.

### 2. Pick the right scan for the job
- ✅ **APPLY:** Use -sS (stealth SYN) for TCP, -sU for UDP services, and scope ports with -p / -F / -p-.
- ⛔ **AVOID:** Blindly running -A -T5 on production — it's loud, heavy, and can disrupt fragile services.

### 3. Add detection, then verify
- ✅ **APPLY:** Layer -sV (versions) and -O (OS) and targeted NSE scripts, then confirm findings manually.
- ⛔ **AVOID:** Treating raw scan output as ground truth — versions and OS guesses can be wrong.

## Cheat Reference — concepts to remember

- **Authorization first** — Only scan networks you own or have explicit written permission to test.
- **Pick the right scan for the job** — Use -sS (stealth SYN) for TCP, -sU for UDP services, and scope ports with -p / -F / -p-.
- **Add detection, then verify** — Layer -sV (versions) and -O (OS) and targeted NSE scripts, then confirm findings manually.

## Interview Questions

#### Q1. What's the difference between a SYN scan and a TCP connect scan?
A SYN scan (-sS) sends SYN and never completes the handshake — faster and stealthier. A connect scan (-sT) completes the full three-way handshake, so it's more detectable and logged.

#### Q2. How do you detect services and OS with Nmap?
-sV probes open ports to fingerprint service versions; -O analyzes TCP/IP stack behavior to guess the OS (needs at least one open and one closed port); -A bundles both plus scripts and traceroute.

#### Q3. What do timing templates (-T0..-T5) do?
They trade speed for stealth/accuracy: T0–T1 are slow and evasive, T3 is the default, T4–T5 are fast and aggressive but noisier and more error-prone.

---

_Curated by Ali Arsalan · https://aliarsalan177.github.io · Generated from the Engineering Guides._
