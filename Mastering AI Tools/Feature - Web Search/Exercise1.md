# Exercise 1: Quick vs Thinking Mode in Test Planning

<p align="right">Date: <strong>2026-03-02</strong></p>

---

## 📝 Task

You are reviewing a QA workflow and need to decide whether to use Web Search for the following scenarios. For each, provide a detailed justification referencing the principles in the article:

-Checking if a new version of your test automation framework was released last week
-Understanding the theoretical differences between boundary value analysis and equivalence partitioning
-Looking up the latest GDPR amendments affecting test data management
-Confirming the default API response format for a legacy system from 2018

---


## ✅ Result

## 1) Checking if a new version of your test automation framework was released last week

**✅ Use Web Search**

### Justification:
- This is **time-sensitive** (“released last week”).
- Software releases are **dynamic and frequently updated**.
- Accurate version information requires checking an **official source** (e.g., GitHub releases, vendor website, changelog).
- According to the principle: use Web Search for *fresh, current, or time-sensitive information*.

Without Web Search, you risk giving outdated release information.

---

## 2) Understanding the theoretical differences between boundary value analysis and equivalence partitioning

**❌ Do NOT use Web Search**

### Justification:
- This is **foundational theoretical knowledge** in software testing.
- The concepts are **stable and well-established**.
- The question is conceptual, not time-sensitive or version-dependent.
- The guidance explicitly says not to use Web Search for:
  - Explaining meanings of terms
  - General concepts or theories

This can be answered directly from established knowledge.

---

## 3) Looking up the latest GDPR amendments affecting test data management

**✅ Use Web Search**

### Justification:
- GDPR regulations can be **updated or interpreted differently over time**.
- Legal and compliance matters are **high-stakes and must be current**.
- The request explicitly asks for the “latest amendments,” which is inherently time-sensitive.
- The policy requires Web Search for:
  - Information that could change over time
  - Current government policies and legal matters
  - High-stakes regulatory information

Providing outdated legal information could lead to compliance risks.

---

## 4) Confirming the default API response format for a legacy system from 2018

**⚖️ Generally, Do NOT Use Web Search (unless documentation is unavailable internally)**

### Justification:
- The system is described as **legacy (from 2018)** — not inherently time-sensitive.
- This is **product-specific technical documentation**, which should ideally come from:
  - Internal documentation
  - Official archived documentation
- The guidelines say not to use Web Search for:
  - General technical explanations
  - Historical information that is not contemporary

However, Web Search *may* be appropriate if:
- Internal documentation is missing
- The API is public and requires verification from archived official docs

By default in a QA workflow context, this should rely on controlled documentation rather than live web lookup.

---

# Final Summary

| Scenario | Use Web Search? | Reason |
|-----------|------------------|---------|
| New framework version released last week | ✅ Yes | Time-sensitive, dynamic information |
| Theoretical differences in test techniques | ❌ No | Stable conceptual knowledge |
| Latest GDPR amendments | ✅ Yes | Legal, high-stakes, time-sensitive |
| Default API response format (2018 system) | ❌ Usually No | Historical/internal documentation |

---