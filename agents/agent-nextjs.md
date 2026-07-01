# Agent Guide — Next.js (App Router)

> React framework with the App Router, Server Components, and file-based routing. Default to the server, fetch on the server, ship less JS.
> Category: Frontend · Source: https://aliarsalan177.github.io/guides/nextjs/

## Purpose

A field guide for **AI coding agents** (and engineers): the concepts to apply and the
mistakes to avoid when building with Next.js (App Router). Load this into your agent's context so it
keeps these concepts in mind and does **not** repeat these mistakes.

## Rules — Apply / Avoid

### 1. Server Components by default
- ✅ **APPLY:** Keep components server-rendered; add 'use client' only where you need state, effects, or browser APIs.
- ⛔ **AVOID:** Slapping 'use client' at the top of the tree — it ships everything to the browser and kills the benefit.

### 2. Fetch data on the server
- ✅ **APPLY:** await data directly in server components; use fetch caching / revalidate and Suspense for streaming.
- ⛔ **AVOID:** useEffect fetch waterfalls for data you could load on the server; leaking secrets into client bundles.

### 3. Use the file-based conventions
- ✅ **APPLY:** Organize with app/ segments: layout, page, loading, error, route handlers; colocate components.
- ⛔ **AVOID:** Fighting the router with ad-hoc structures; putting server-only code in client components.

### 4. Mutate with Server Actions / Route Handlers
- ✅ **APPLY:** Use server actions or route handlers for mutations; revalidatePath/Tag to refresh cached data.
- ⛔ **AVOID:** Building throwaway API endpoints for everything when a server action fits; forgetting to revalidate.

### 5. Optimize assets and metadata
- ✅ **APPLY:** next/image, next/font, and the Metadata API (+ generateStaticParams for static export).
- ⛔ **AVOID:** Unoptimized <img>, layout-shifting fonts, and missing SEO metadata.

## Cheat Reference — concepts to remember

- **Server Components by default** — Keep components server-rendered; add 'use client' only where you need state, effects, or browser APIs.
- **Fetch data on the server** — await data directly in server components; use fetch caching / revalidate and Suspense for streaming.
- **Use the file-based conventions** — Organize with app/ segments: layout, page, loading, error, route handlers; colocate components.
- **Mutate with Server Actions / Route Handlers** — Use server actions or route handlers for mutations; revalidatePath/Tag to refresh cached data.
- **Optimize assets and metadata** — next/image, next/font, and the Metadata API (+ generateStaticParams for static export).

## Full Cheat Sheet — every concept

### App Router Files
- app/segment/: layout.tsx, page.tsx, loading.tsx, error.tsx, not-found.tsx, route.ts, template.tsx.
- Dynamic segments [id], catch-all [...slug], route groups (group), private _folder.
- generateStaticParams + generateMetadata for static/SEO.

### Rendering
- Server Components default (no JS shipped); 'use client' for interactivity.
- Streaming with Suspense + loading.js; partial prerendering.
- SSG / SSR / ISR (revalidate); output: 'export' for fully static sites.

### Data & Mutations
- await fetch in server components; caching via { cache, next: { revalidate, tags } }.
- Server Actions ('use server') for mutations; Route Handlers (route.ts) for APIs.
- revalidatePath / revalidateTag to refresh cached data.

### Optimization & Config
- next/image (responsive, lazy), next/font (no layout shift), next/script.
- Metadata API + JSON-LD; Middleware (edge) for auth/redirects.
- Env: NEXT_PUBLIC_ exposed to client; everything else server-only.

## Interview Questions

#### Q1. Server Components vs Client Components?
Server Components render on the server with zero JS shipped and can fetch data/secrets directly; Client Components ('use client') run in the browser for interactivity (state, effects, events). Default to server, opt into client at the leaves.

#### Q2. How does data fetching work in the App Router?
You await data directly in async server components. fetch is cached by default with control via { cache, next: { revalidate, tags } }; Suspense + loading.js stream UI while data loads.

#### Q3. What are Server Actions?
Async functions marked 'use server' that run on the server and can be called from components/forms to mutate data without hand-writing an API route, then revalidatePath/revalidateTag to refresh caches.

#### Q4. SSG vs SSR vs ISR?
Static Generation prerenders at build (fast, cacheable); SSR renders per request (fresh, dynamic); Incremental Static Regeneration rebuilds static pages on a revalidate interval — best of both for content that changes occasionally.

#### Q5. How do special files work (layout, page, loading, error)?
Each route segment can have layout.tsx (shared UI/state across children), page.tsx (the route UI), loading.tsx (Suspense fallback), error.tsx (error boundary), and route.ts (API handler).

---

_Curated by Ali Arsalan · https://aliarsalan177.github.io · Generated from the Engineering Guides._
