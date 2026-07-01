# Agent Guide — Unity (Game Dev)

> Cross-platform game engine driven by C#. Think in GameObjects, Components, and the frame lifecycle — and keep the update loop cheap.
> Category: Frontend · Source: https://aliarsalan177.github.io/guides/unity/

## Purpose

A field guide for **AI coding agents** (and engineers): the concepts to apply and the
mistakes to avoid when building with Unity (Game Dev). Load this into your agent's context so it
keeps these concepts in mind and does **not** repeat these mistakes.

## Rules — Apply / Avoid

### 1. Compose with GameObjects + Components
- ✅ **APPLY:** Build behavior by attaching Components (scripts, colliders, renderers) to GameObjects — composition over inheritance.
- ⛔ **AVOID:** Giant monolithic MonoBehaviours that do everything — hard to reuse, test, and reason about.

### 2. Use the right lifecycle method
- ✅ **APPLY:** Awake/Start for setup, Update for per-frame logic, FixedUpdate for physics; cache references in Awake.
- ⛔ **AVOID:** Physics in Update or heavy per-frame allocation — it causes frame-rate-dependent bugs and GC spikes.

### 3. Keep Update() cheap
- ✅ **APPLY:** Cache GetComponent/Find results, pool objects, and avoid allocations in hot loops.
- ⛔ **AVOID:** Calling GetComponent/Find or instantiating/destroying every frame — it tanks performance and triggers GC.

### 4. Frame-rate-independent movement
- ✅ **APPLY:** Multiply movement/timers by Time.deltaTime so behavior is consistent across frame rates.
- ⛔ **AVOID:** Moving by a fixed amount per frame — speed then depends on the player's FPS.

### 5. Pool instead of Instantiate/Destroy
- ✅ **APPLY:** Reuse objects from an object pool for bullets/enemies/particles.
- ⛔ **AVOID:** Constant Instantiate()/Destroy() at runtime — it fragments memory and stutters via garbage collection.

## Cheat Reference — concepts to remember

- **Compose with GameObjects + Components** — Build behavior by attaching Components (scripts, colliders, renderers) to GameObjects — composition over inheritance.
- **Use the right lifecycle method** — Awake/Start for setup, Update for per-frame logic, FixedUpdate for physics; cache references in Awake.
- **Keep Update() cheap** — Cache GetComponent/Find results, pool objects, and avoid allocations in hot loops.
- **Frame-rate-independent movement** — Multiply movement/timers by Time.deltaTime so behavior is consistent across frame rates.
- **Pool instead of Instantiate/Destroy** — Reuse objects from an object pool for bullets/enemies/particles.

## Full Cheat Sheet — every concept

### Core Concepts
- GameObject + Components; Transform (position/rotation/scale); Scenes; Prefabs (reusable templates).
- Scripting in C# via MonoBehaviour; ScriptableObjects for data assets.

### Lifecycle
- Awake → OnEnable → Start (setup); Update (per frame); FixedUpdate (physics); LateUpdate (post).
- OnCollisionEnter/OnTriggerEnter for collisions; OnDestroy for cleanup.

### Physics & Input
- Rigidbody + Colliders; apply forces in FixedUpdate; isTrigger for overlap detection.
- Input System (or legacy Input); raycasting; layers & tags.

### Performance
- Cache GetComponent/references; object pooling; minimize allocations & GC.
- Time.deltaTime for frame independence; batch draw calls; use the Profiler.

### Systems & Build
- Coroutines (yield) for timed sequences; Animator/animation; UI (Canvas).
- Render pipelines (URP/HDRP); asset bundles/Addressables; build to PC/mobile/console/WebGL.

## Interview Questions

#### Q1. What are GameObjects and Components?
A GameObject is a container in the scene; Components (Transform, Rigidbody, Collider, custom MonoBehaviour scripts) attach to it to give behavior and data. Unity favors composition — you build entities by combining components.

#### Q2. Update vs FixedUpdate vs LateUpdate?
Update runs once per frame (variable timestep — gameplay/input). FixedUpdate runs at a fixed timestep (physics/Rigidbody). LateUpdate runs after all Updates (camera follow). Physics goes in FixedUpdate.

#### Q3. Why multiply by Time.deltaTime?
deltaTime is the seconds since the last frame; multiplying movement/timers by it makes behavior frame-rate independent, so speed is consistent whether running at 30 or 144 FPS.

#### Q4. How do you optimize a Unity game?
Cache component lookups, pool objects instead of Instantiate/Destroy, minimize per-frame allocations (avoid GC), batch draw calls, use the Profiler, and offload heavy work (jobs/coroutines).

#### Q5. What are Prefabs and ScriptableObjects?
Prefabs are reusable, instantiable GameObject templates. ScriptableObjects are data containers stored as assets — great for config/shared data without attaching to a scene object, reducing duplication.

---

_Curated by Ali Arsalan · https://aliarsalan177.github.io · Generated from the Engineering Guides._
