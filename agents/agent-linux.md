# Agent Guide — Linux Command Line

> The operator's toolkit: navigate, permission, pipe, and manage processes with confidence — carefully.
> Category: Backend & Infra · Source: https://aliarsalan177.github.io/guides/linux/

## Purpose

A field guide for **AI coding agents** (and engineers): the concepts to apply and the
mistakes to avoid when building with Linux Command Line. Load this into your agent's context so it
keeps these concepts in mind and does **not** repeat these mistakes.

## Rules — Apply / Avoid

### 1. Compose small tools with pipes
- ✅ **APPLY:** Chain commands with | and redirect with > / >> / 2>&1 to build powerful one-liners (grep | sort | uniq).
- ⛔ **AVOID:** Parsing structured data with fragile ad-hoc text hacks when a proper tool (jq, awk) fits better.

### 2. Respect destructive commands
- ✅ **APPLY:** Double-check paths; prefer rm -i, and test globs with ls before rm -rf.
- ⛔ **AVOID:** rm -rf on an unverified or variable path (rm -rf $DIR/ with empty $DIR) — it's irreversible.

### 3. Least-privilege permissions
- ✅ **APPLY:** Set the minimum chmod/chown needed; use sudo deliberately for single commands.
- ⛔ **AVOID:** chmod 777 to 'make it work' — it opens files to everyone and is a security hole.

### 4. Manage processes and services
- ✅ **APPLY:** Inspect with ps aux/top, signal with kill, and run services via systemctl; schedule with cron.
- ⛔ **AVOID:** kill -9 as a first resort — it skips cleanup; try a graceful signal first.

## Cheat Reference — concepts to remember

- **Compose small tools with pipes** — Chain commands with | and redirect with > / >> / 2>&1 to build powerful one-liners (grep | sort | uniq).
- **Respect destructive commands** — Double-check paths; prefer rm -i, and test globs with ls before rm -rf.
- **Least-privilege permissions** — Set the minimum chmod/chown needed; use sudo deliberately for single commands.
- **Manage processes and services** — Inspect with ps aux/top, signal with kill, and run services via systemctl; schedule with cron.

## Full Cheat Sheet — every concept

### Navigation & Files
- ls (-l -a -h), cd (~ home, - previous), pwd.
- mkdir -p, cp -r, mv -i, rm -r (rm -rf forces), touch.

### Permissions
- chmod symbolic or octal (r=4 w=2 x=1): 755 dirs/execs, 644 files.
- chown user:group file; least privilege — avoid chmod 777.

### Viewing & Searching
- cat, less/more, head/tail; grep (-i -n -r); find by name/size/type.

### Processes & Services
- ps aux, top; kill <pid> (SIGTERM), kill -9 last resort.
- systemctl start/stop/restart/enable; schedule with cron (crontab -e).

### Pipes, Redirection, Net
- | pipe; > overwrite, >> append, 2>&1 merge stderr, < input.
- ssh, scp, curl/wget; apt/dpkg install/update; man <cmd> for docs.

### Jobs, Disk & SSH Hardening
- Job control: cmd & (background), Ctrl+Z + bg/fg, jobs, nohup, disown.
- Disk/system: df -h, du -sh, free -h, uname -a, top/htop, uptime.
- SSH hardening: PermitRootLogin no, change Port, AllowUsers, key-based auth; firewall (ufw/iptables).

### Bash Scripting
- Variables ($VAR, $(cmd)), aliases, history (!!, Ctrl+R).
- if [ cond ]; then … fi; for/while loops; test operators (-f, -d, -z, -eq).
- cron for scheduling: `min hour dom mon dow command` (crontab -e).

## Interview Questions

#### Q1. What do chmod 755 and 644 mean?
Three octal digits for owner/group/others where r=4, w=2, x=1. 755 = owner rwx, group/others r-x (typical for executables/dirs); 644 = owner rw, others r-- (typical for files).

#### Q2. Explain pipes and redirection.
`|` sends one command's stdout to the next command's stdin; `>` writes stdout to a file (overwrite), `>>` appends, and `2>&1` merges stderr into stdout so both are captured together.

#### Q3. How do you find and kill a process?
Locate it with ps aux | grep name or top, note the PID, then kill <pid> (graceful SIGTERM). Use kill -9 (SIGKILL) only if it won't terminate, since it skips cleanup.

---

_Curated by Ali Arsalan · https://aliarsalan177.github.io · Generated from the Engineering Guides._
