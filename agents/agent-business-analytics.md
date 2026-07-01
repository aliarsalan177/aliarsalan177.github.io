# Agent Guide — Business Analytics

> Turn data into decisions. Know your analytics type, clean your data, and never confuse correlation with causation.
> Category: AI & Data · Source: https://aliarsalan177.github.io/guides/business-analytics/

## Purpose

A field guide for **AI coding agents** (and engineers): the concepts to apply and the
mistakes to avoid when building with Business Analytics. Load this into your agent's context so it
keeps these concepts in mind and does **not** repeat these mistakes.

## Rules — Apply / Avoid

### 1. Match the analytics type to the question
- ✅ **APPLY:** Descriptive (what happened), diagnostic (why), predictive (what will), prescriptive (what to do) — pick before you model.
- ⛔ **AVOID:** Jumping to prediction on dirty or unrepresentative data — garbage in, confident garbage out.

### 2. Correlation is not causation
- ✅ **APPLY:** Establish causality with experiments (A/B tests) or quasi-experimental methods (diff-in-diff, matching).
- ⛔ **AVOID:** Acting on a correlation as if it were causal — confounders will burn you.

### 3. Choose robust summary statistics
- ✅ **APPLY:** Use the median for skewed data, report dispersion (std dev), and check distribution before applying tests.
- ⛔ **AVOID:** Reporting only the mean on skewed/outlier-heavy data — it misleads.

### 4. Measure what matters
- ✅ **APPLY:** Define a few actionable KPIs tied to outcomes; segment (RFM, cohorts, funnels) to find the story.
- ⛔ **AVOID:** Vanity metrics and cluttered dashboards that look busy but drive no decision.

## Cheat Reference — concepts to remember

- **Match the analytics type to the question** — Descriptive (what happened), diagnostic (why), predictive (what will), prescriptive (what to do) — pick before you model.
- **Correlation is not causation** — Establish causality with experiments (A/B tests) or quasi-experimental methods (diff-in-diff, matching).
- **Choose robust summary statistics** — Use the median for skewed data, report dispersion (std dev), and check distribution before applying tests.
- **Measure what matters** — Define a few actionable KPIs tied to outcomes; segment (RFM, cohorts, funnels) to find the story.

## Full Cheat Sheet — every concept

### Types of Analytics
- Descriptive (what happened) · Diagnostic (why) · Predictive (what's next) · Prescriptive (what to do).

### Statistics
- Central tendency: mean (symmetric), median (robust to skew), mode (categorical).
- Dispersion: standard deviation vs the mean; correlation ∈ [-1, +1].
- Check distribution/normality before applying tests.

### Inference & Causality
- Null (H0) vs alternative (H1) hypothesis; p-value < 0.05 → reject H0 (statistically significant).
- Tests: t-test (continuous), chi-square (categorical), Shapiro-Wilk (normality); confidence intervals & standard error of the mean.
- z-score = how many std devs from the mean; normal distribution 68-95-99.7 rule.
- Regression: linear (continuous), logistic (binary — read odds/coefficients), multi-linear (many predictors).
- Correlation ≠ causation — establish causality with A/B tests, diff-in-diff, matching.
- Python: pandas, NumPy, SciPy, statsmodels, scikit-learn.

### Segmentation & Modeling
- RFM (recency/frequency/monetary), clustering, cohorts, funnels.
- Random forest (ensemble), time-series forecasting (e.g. Prophet).

### Practice
- Define a few actionable KPIs; clean data first; visualize clearly.
- Avoid vanity metrics and cluttered dashboards.

## Interview Questions

#### Q1. What are the four types of analytics?
Descriptive (what happened), diagnostic (why it happened), predictive (what's likely next), and prescriptive (what action to take). Each answers a different business question and needs different methods.

#### Q2. Why is correlation not causation?
Two variables can move together due to a confounder or coincidence without one causing the other. Establishing causation needs controlled experiments (A/B tests) or quasi-experimental designs, not correlation alone.

#### Q3. Mean vs median — when to use which?
The mean is the balance point for roughly symmetric data; the median is robust to outliers and skew, so it better represents 'typical' values in skewed distributions like income or response times.

#### Q4. What is an A/B test?
A randomized experiment splitting users into control and variant to isolate the causal effect of a change, measured on a predefined metric with statistical significance before rolling out.

---

_Curated by Ali Arsalan · https://aliarsalan177.github.io · Generated from the Engineering Guides._
