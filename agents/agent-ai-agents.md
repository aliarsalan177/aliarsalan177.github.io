# Agent Guide — AI, ML & LLM Engineering

> From ML/DL foundations (models, metrics, neural nets, transformers) to prompting, RAG, tools and the agent loop. Ground your models, constrain your outputs, and budget your steps.
> Category: AI & Data · Source: https://aliarsalan177.github.io/guides/ai-agents/

## Purpose

A field guide for **AI coding agents** (and engineers): the concepts to apply and the
mistakes to avoid when building with AI, ML & LLM Engineering. Load this into your agent's context so it
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

### ML / DL Foundations
- Pipeline: data → features (X) / labels (y) → model → loss → optimization → evaluation → deployment.
- Split train / validation / test; touch test once at the end. For time series, split chronologically.
- Loss = 'badness' score to minimize (usually via gradient descent); objective = generalize to unseen data.
- Underfitting (too simple) vs overfitting (memorizes noise); the bias–variance trade-off.
- Beware data leakage: no train info that wouldn't exist at prediction time.

### Types of Learning
- Supervised (labeled): classification, regression. Unsupervised (no labels): clustering, dimensionality reduction.
- Semi-supervised (some labels), Self-supervised (labels from the data itself — foundation of modern DL).
- Reinforcement learning (trial-and-error with rewards); online/continual (adapt to streams, watch drift).

### Classic ML Models
- Linear/Logistic regression (fast, interpretable baseline); kNN (simple, struggles in high dims).
- Decision trees → Random forests (robust tabular default) → Gradient boosting (XGBoost/LightGBM/CatBoost, SOTA tabular).
- SVM (kernels), Naive Bayes (text baseline); clustering (k-means, DBSCAN); PCA / t-SNE / UMAP.
- Rule of thumb: tabular → tree-based; images/audio/text → deep learning.

### Evaluation Metrics
- Classification: accuracy (bad with imbalance), precision/recall, F1, ROC-AUC, PR-AUC (rare positives).
- Regression: MAE (robust), RMSE (penalizes big errors), R².
- Ranking/retrieval: NDCG, MAP, MRR. Use a confusion matrix to surface mistakes.

### Deep Learning
- Neuron: a = φ(w·x + b); activations: ReLU (default), sigmoid, tanh, GELU (transformers).
- Architectures: MLP (tabular), CNN (images), RNN/LSTM/GRU (sequences), Transformers (SOTA language/vision).
- Backprop = chain-rule gradients; update = param − learning_rate × gradient.
- Transformers: self-attention (queries/keys/values) + positional encodings = global context, parallelizable.
- Embeddings: dense vectors capturing semantic similarity (cosine similarity ≈ meaning).

### Prompt Framework (R-O-C-F-E-E)
- Role → Objective → Constraints → Format → Examples → Edge cases.
- Keep the single source of truth (key rules) in the system message / at the very top.
- 10 patterns cover ~90%: instruction/format, classification, extraction, summarization, grounded QA, rewriting, reasoning, tool-use, comparison, transformation.

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

#### Q6. Explain overfitting and the bias–variance trade-off.
Overfitting is when a model memorizes training noise and fails to generalize (low bias, high variance); underfitting is too simple (high bias, low variance). You detect overfitting with a validation set and fight it with more/cleaner data, regularization, dropout, and early stopping.

#### Q7. Supervised vs unsupervised learning?
Supervised learns from labeled examples (classification, regression); unsupervised finds structure in unlabeled data (clustering, dimensionality reduction). Self-supervised creates labels from the data itself and underpins modern deep learning.

#### Q8. Precision vs recall vs F1 — when do they matter?
Precision = of predicted positives, how many are correct (cost of false positives); recall = of actual positives, how many you caught (cost of false negatives); F1 is their harmonic mean. Accuracy misleads under class imbalance — prefer these or PR-AUC.

#### Q9. How do transformers work at a high level?
Self-attention lets each token weigh the relevance of every other token (via queries, keys, values), with positional encodings adding order. This gives global context with parallel computation, outperforming RNNs on long sequences — the basis of modern LLMs.

#### Q10. When would you use classic ML vs deep learning?
For tabular data, tree-based models (random forests, gradient boosting) are usually the strong default; for images, audio, and text, deep learning (CNNs, transformers) that learns features from raw data typically wins.

---

_Curated by Ali Arsalan · https://aliarsalan177.github.io · Generated from the Engineering Guides._
