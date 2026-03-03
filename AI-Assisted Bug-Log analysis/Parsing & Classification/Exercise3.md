# 🧪 Exercise 3: Analyze Playwright Test Failures (UI Locator Issues)

<p align="right">Date: <strong>2026-03-03</strong></p>

---

## 📝 Task

Scenario:

Your team uses TypeScript + Playwright to test a Pet Insurance web application. A UI regression suite has started failing after recent UI changes. Several tests fail with element-related errors.

**Sample Playwright Test Logs:**
```text
[2025-10-12 11:02:15] ▶ Running 18 tests using 3 workers
[2025-10-12 11:02:18] ✖ QuoteFlow.spec.ts › should calculate insurance price
Error: locator.click: strict mode violation: locator('button[data-testid="get-quote"]') resolved to 2 elements
    at QuotePage.getQuote (QuotePage.ts:45)

[2025-10-12 11:02:22] ✖ ClaimFlow.spec.ts › should submit claim successfully
Error: locator.fill: No element found for selector '#claim-amount'
    at ClaimPage.fillAmount (ClaimPage.ts:32)

[2025-10-12 11:02:25] ✖ ClaimFlow.spec.ts › should show claim confirmation
Error: expect(received).toBeVisible()
Locator: text=Claim submitted successfully
Expected: visible
Received: hidden
    at ClaimFlow.spec.ts:67

[2025-10-12 11:02:30] ✓ Login.spec.ts › user can log in
[2025-10-12 11:02:32] ✓ Dashboard.spec.ts › policy summary is visible

[2025-10-12 11:02:35] ▶ Test run finished: 15 passed, 3 failed
```
**Your Task:**

- Paste the logs into ChatGPT.
- Ask AI to group failures by root symptom (not by test name).
- Identify whether failures are likely caused by:
  1. test instability,
  2. UI changes,
  3. application bug.
- Decide which failures should block the release.



# 🧾 Result

# 🧪 Playwright E2E Failure Analysis Report

**Date:** 2025-10-12  
**Framework:** Playwright (TypeScript)  
**Execution:** 18 tests, 3 workers  
**Result:** 15 Passed | 3 Failed  

---

# 📌 Objective

- Group failures by **root symptom** (not by test name)
- Classify each failure as:
  - Test Instability
  - UI Change
  - Application Bug
- Decide which failures should **block the release**

---

# 🔎 Failure Grouping by Root Symptom

---

## 🟥 Root Symptom 1: Strict Mode Violation (Duplicate Selector Match)

### ❌ Error

locator.click: strict mode violation:
locator('button[data-testid="get-quote"]') resolved to 2 elements
at QuotePage.getQuote (QuotePage.ts:45)


### 🔍 What Happened
Playwright strict mode requires a single matching element.  
The selector returned **2 elements**, causing the click to fail.

### 🧠 Likely Cause
- UI update introduced a second `data-testid="get-quote"` button
- Duplicate CTA added (header/footer/modal)
- Test assumes uniqueness which no longer exists

### 🎯 Classification
🟡 **UI Change (Breaking Test Contract)**

### 🧪 Test Instability?
❌ No  
Deterministic failure. Selector consistently resolves to 2 elements.

### 🚦 Release Impact
🟡 **Conditional**

Block release if:
- Duplicate button is unintended
- One button is non-functional
- UX confusion introduced

Do not block if:
- Both buttons work correctly
- Change was intentional
- Test selector needs refinement

---

## 🟥 Root Symptom 2: Missing `#claim-amount` Input Field

### ❌ Error

locator.fill: No element found for selector '#claim-amount'
at ClaimPage.fillAmount (ClaimPage.ts:32)


### 🔍 What Happened
Playwright could not find the claim amount input in the DOM.

### 🧠 Likely Causes
- Field ID renamed (UI refactor)
- Component replaced (design system migration)
- Conditional rendering logic changed
- Application bug preventing field render

### 🎯 Classification
🟡 **UI Change OR**
🔴 **Application Bug**

### 🧪 Test Instability?
❌ No  
Element is completely missing, not timing-related.

### 🚦 Release Impact
🔴 **BLOCK RELEASE**

### Why?
- Claim amount is a **core business input**
- Users cannot submit claims without it
- High risk production regression

---

## 🟥 Root Symptom 3: Confirmation Message Hidden

### ❌ Error

expect(received).toBeVisible()
Locator: text=Claim submitted successfully
Expected: visible
Received: hidden
at ClaimFlow.spec.ts:67


### 🔍 What Happened
Confirmation text exists but is **hidden**.

### 🧠 Likely Causes
- Claim submission did not complete successfully
- Success state not triggered
- Downstream failure from missing claim amount
- Business logic regression

### 🎯 Classification
🔴 **Application Bug**

### 🧪 Test Instability?
❌ No  
Deterministic visibility failure.

### 🚦 Release Impact
🔴 **BLOCK RELEASE**

### Why?
- Claim flow completion broken
- No user confirmation displayed
- Critical workflow failure

---

# 📊 Consolidated Failure Summary

| Root Symptom | Failure Count | Likely Cause | Test Instability | Release Blocker |
|--------------|--------------|--------------|------------------|-----------------|
| Duplicate `data-testid` (strict mode violation) | 1 | UI Change | ❌ No | 🟡 Conditional |
| Missing claim amount field | 1 | UI Change / App Bug | ❌ No | 🔴 Yes |
| Confirmation message hidden | 1 | Application Bug | ❌ No | 🔴 Yes |

---

# 🧠 Stability Assessment

No evidence of:
- Flaky timing issues
- Async race conditions
- Network instability
- Intermittent failures

All failures are **deterministic and reproducible**.

### Conclusion
❌ Not test instability  
✅ Likely real regression in Claim workflow  

---

# 🏁 Release Recommendation

## 🔴 BLOCK RELEASE

### Justification
The **Claim flow** is broken in a core business scenario:

- Claim amount input not found
- Success confirmation not visible

This represents a high-severity production risk.

---

# 🏆 Executive Summary

- ✅ Login flow stable
- ✅ Dashboard stable
- 🟡 Quote flow needs selector refinement (likely UI change)
- 🔴 Claim flow regression detected
- ❌ No flakiness observed

**Final Decision: Release Blocked**