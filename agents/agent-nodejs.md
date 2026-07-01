# Agent Guide — Node.js

> Event-driven, non-blocking JavaScript on the server. Keep the loop free, handle every rejection, and stream big data.
> Category: Backend & Infra · Source: https://aliarsalan177.github.io/guides/nodejs/

## Purpose

A field guide for **AI coding agents** (and engineers): the concepts to apply and the
mistakes to avoid when building with Node.js. Load this into your agent's context so it
keeps these concepts in mind and does **not** repeat these mistakes.

## Rules — Apply / Avoid

### 1. Never block the event loop
- ✅ **APPLY:** Use async APIs; offload CPU-heavy work to worker threads or a queue.
- ⛔ **AVOID:** Sync calls (fs.readFileSync, big JSON.parse, crypto in a request path) — they stall every other request.

### 2. Handle every async error
- ✅ **APPLY:** await inside try/catch, attach .catch to promises, and listen for 'error' on streams/emitters.
- ⛔ **AVOID:** Unhandled promise rejections — they can crash the process and leave requests hanging.

### 3. Stream large payloads
- ✅ **APPLY:** Pipe files/HTTP bodies through streams so memory stays flat regardless of size.
- ⛔ **AVOID:** Buffering a whole file/response into memory — it spikes RAM and falls over under load.

### 4. Config via environment, secrets out of code
- ✅ **APPLY:** Read config from process.env; commit package-lock.json for reproducible installs.
- ⛔ **AVOID:** Hard-coding secrets or shipping them in package.json / the repo.

## Cheat Reference — concepts to remember

- **Never block the event loop** — Use async APIs; offload CPU-heavy work to worker threads or a queue.
- **Handle every async error** — await inside try/catch, attach .catch to promises, and listen for 'error' on streams/emitters.
- **Stream large payloads** — Pipe files/HTTP bodies through streams so memory stays flat regardless of size.
- **Config via environment, secrets out of code** — Read config from process.env; commit package-lock.json for reproducible installs.

## Full Cheat Sheet — every concept

### Runtime & Event Loop
- Node runs V8 + libuv; JS is single-threaded but I/O is delegated (thread pool / OS) and non-blocking.
- The event loop schedules callbacks when operations complete — many connections without a thread each.
- Never block the loop with sync CPU work; offload to worker threads or a queue.

### Modules & npm
- CommonJS: require() / module.exports (sync). ESM: import/export (.mjs or "type":"module").
- package.json defines the project; commit package-lock.json for reproducible installs.
- Keep secrets out of package.json and the repo.

### Core Modules & Globals
- fs (files), http/https (servers), path (OS-safe paths), process (argv/env), events (EventEmitter), crypto, stream, os.
- Globals: global, process, __dirname, __filename, Buffer, setTimeout/setInterval; node REPL for quick eval.

### EventEmitter & Streams
- EventEmitter: emitter.on('event', cb) / emitter.emit('event'); the backbone of many Node APIs.
- Streams process data in chunks (readable/writable/duplex/transform); pipe() large data to keep memory flat.
- Buffers hold binary data.

### Async & Errors
- Prefer async/await; await inside try/catch and attach .catch to promises.
- Listen for 'error' on streams/emitters; unhandled rejections can crash the process.

### Config, Scaling, Express
- Read config from process.env per environment.
- Scale across cores with the cluster module / PM2 / worker_threads.
- Express maps HTTP verbs to routes (GET/POST/PUT/DELETE) with middleware; close DB connections properly.

## Interview Questions

#### Q1. How does Node handle concurrency if it's single-threaded?
JS runs on one thread, but I/O is delegated to libuv's thread pool and the OS; the event loop schedules callbacks when operations complete. This non-blocking model handles many connections without a thread per request.

#### Q2. What happens if you block the event loop?
All other callbacks — including incoming requests — wait. A single synchronous CPU-bound or sync-I/O call freezes the whole server, so such work must be async, chunked, or moved to worker threads.

#### Q3. Why prefer streams?
Streams process data in chunks, keeping memory usage constant and starting output sooner, which is essential for large files, uploads, and proxying without exhausting RAM.

#### Q4. CommonJS vs ES Modules?
CommonJS uses require/module.exports and loads synchronously (Node's classic system); ESM uses import/export, is statically analyzable and async-friendly. Pick one per project and stay consistent.

---

_Curated by Ali Arsalan · https://aliarsalan177.github.io · Generated from the Engineering Guides._
