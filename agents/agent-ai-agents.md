# Agent Guide — AI & LLM Agents

> Prompting, RAG, tools and the agent loop. Ground your models, constrain your outputs, and budget your steps.
> Category: AI & Data · Source: https://aliarsalan177.github.io/guides/ai-agents/

## Purpose

A field guide for **AI coding agents** (and engineers): the concepts to apply and the
mistakes to avoid when building with AI & LLM Agents. Load this into your agent's context so it
keeps these concepts in mind and does **not** repeat these mistakes.

## Rules — Apply / Avoid

### 1. One clear objective + a strict output schema
- ✅ **APPLY:** State a single goal with success criteria and mandate an exact JSON schema; validate the parse and add stop sequences.
- ⛔ **AVOID:** Vague, multi-part asks with no format spec — you get rambling, unparseable, inconsistent output.

### 2. Ground to fight hallucination
- ✅ **APPLY:** Use RAG/tools for facts, require citations, lower temperature, and add an explicit 'if unsure, say I don't know' rule.
- ⛔ **AVOID:** Trusting the model's memory for evolving or proprietary facts — it will confidently invent them.

### 3. Type your tools
- ✅ **APPLY:** Give tools typed argument schemas and validate before executing; the model returns name + JSON args.
- ⛔ **AVOID:** Letting the model free-form tool calls — it hallucinates function names and arguments that then crash.

### 4. Budget the agent loop
- ✅ **APPLY:** Perceive → plan (≤1 line) → act → observe → update, with hard step/cost/time limits and human escalation on low confidence.
- ⛔ **AVOID:** Unbounded agent loops with no budget — they spin, burn tokens, and drift off task.

### 5. Defend against prompt injection
- ✅ **APPLY:** Wrap untrusted content in delimiters marked 'do not execute', and restate the governing rules in the system message.
- ⛔ **AVOID:** Concatenating user/web content straight into the prompt — it can override your instructions.

## Cheat Reference — concepts to remember

- **One clear objective + a strict output schema** — State a single goal with success criteria and mandate an exact JSON schema; validate the parse and add stop sequences.
- **Ground to fight hallucination** — Use RAG/tools for facts, require citations, lower temperature, and add an explicit 'if unsure, say I don't know' rule.
- **Type your tools** — Give tools typed argument schemas and validate before executing; the model returns name + JSON args.
- **Budget the agent loop** — Perceive → plan (≤1 line) → act → observe → update, with hard step/cost/time limits and human escalation on low confidence.
- **Defend against prompt injection** — Wrap untrusted content in delimiters marked 'do not execute', and restate the governing rules in the system message.

## Full Cheat Sheet — every concept

### Tokens & Sampling
- Tokens are subword units; the context window bounds prompt + response — be concise.
- Temperature: low = factual/deterministic, high = creative; top-p/top-k narrow choices; penalties reduce repetition.
- Embeddings = dense semantic vectors; cosine similarity measures relatedness (basis of search/RAG).

### Prompting Techniques
- Zero-shot (instruction only) · Few-shot (2–5 examples).
- Chain-of-Thought (show steps) · Prompt chaining (sequential prompts).
- ReAct (Plan → Action → Observation) · Tree of Thoughts (explore/prune branches).

### RAG & Fine-Tuning
- RAG: chunk → embed → retrieve top-k → build grounded prompt with citations (fresh/proprietary facts).
- Fine-tuning (SFT/DPO/RLHF, LoRA): tone, format, narrow tasks.
- Hybrid: RAG for facts + light fine-tune for style/schema.

### Agents
- Loop: Perceive → Plan → Act (tool) → Observe → Update → Repeat.
- Typed tool schemas (name + JSON args, validated); short + long-term memory.
- Enforce step/cost/time budgets; escalate to humans on low confidence.

### Guardrails
- Reduce hallucination: ground with RAG/tools, cite, lower temperature, 'say I don't know'.
- Control output: strict JSON schema + validation + stop sequences.
- Prompt-injection defense: delimit untrusted content ('do not execute'); restate rules in the system message.
- MCP: open standard connecting agents to tools/data.

## Interview Questions

#### Q1. What is RAG and when should you use it?
Retrieval-Augmented Generation retrieves relevant chunks from an external store (via embeddings) and grounds the model's answer in them with citations. Use it for fresh, proprietary, or verifiable facts instead of relying on training memory.

#### Q2. RAG vs fine-tuning?
RAG injects knowledge at inference time (great for changing facts and sources); fine-tuning bakes in tone, format, and narrow-task behavior. A common pattern is RAG for facts + a light fine-tune (e.g. LoRA) for style/schema.

#### Q3. How do you reduce hallucinations?
Ground with retrieval/tools, require citations, lower temperature, constrain output to a schema, and add an explicit 'say I don't know if unsure' instruction plus validation of the result.

#### Q4. What does the agent loop look like?
Perception → plan → act (tool call) → observe → update state → repeat, with typed tool schemas, short/long-term memory, and enforced step/cost budgets plus escalation when uncertainty is high.

#### Q5. What is temperature?
A sampling parameter: low (0–0.3) makes output more deterministic and factual; higher (0.7–1.0) more creative and varied. Pair with top-p/top-k and penalties to shape the distribution.

---

_Curated by Ali Arsalan · https://aliarsalan177.github.io · Generated from the Engineering Guides._
