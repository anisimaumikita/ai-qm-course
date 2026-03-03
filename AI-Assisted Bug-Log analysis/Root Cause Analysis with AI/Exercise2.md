# 🧪 Exercise 2: UI Failure → Backend Pricing Root Cause (Pet Insurance)

<p align="right">Date: <strong>2026-03-03</strong></p>

---

## 📝 Task

**Goal:** Determine whether a failing UI Playwright test is caused by a UI locator issue or by a backend pricing regression.

**Steps to implement:**

1. Paste the logs into ChatGPT.
2. Try creating a prompt yourself first (don’t look at the suggested prompt yet).
3. Ask AI to:
  - identify the most likely root cause,
  - rank hypotheses by likelihood,
  - propose 2–3 validation steps,
  - suggest what to include in the bug report.
4. Compare your result with the Expected Result.

### 🧾 Result

# Playwright Test Failure Analysis – Pet Insurance Pricing

## 1) Most Likely Root Cause (One Sentence)

The test expected the old Labrador premium ($45), but after deploying pricing-service v3.8.0 with RiskTableVersion=2025.10.14-1 (Labrador factor changed), the pricing engine correctly recalculated the premium to $52, causing a stale assertion.

---

## 2) Hypotheses (Ranked by Likelihood)

### 🥇 1. Risk table update changed Labrador factor (Most Likely)

**Evidence:**
- Log: `Updated risk table: 2025.10.14-1 (Labrador factor changed)`
- Pricing log: `breedRiskFactor=1.15 base=40 computedPremium=52`
- Deployment occurred at 09:00; failure at 09:12
- Expected value ($45) likely aligned with previous factor

Strong indication test expectation was not updated after intentional pricing change.

---

### 🥈 2. Test environment unintentionally picked up new risk table version

**Evidence:**
- RiskTableVersion logged explicitly as `2025.10.14-1`
- No indication of pricing-service mocking
- Test appears to call real `/api/quote`

If the test assumes static pricing, environment drift could cause failure.

---

### 🥉 3. Additional pricing logic changed in v3.8.0

**Evidence:**
- pricing-service v3.8.0 deployed at same time as risk table update
- 40 × 1.15 ≠ 52 exactly, suggesting additional load/fees/rounding logic
- Possible surcharge or minimum premium logic introduced

Less likely because logs explicitly mention Labrador factor change.

---

## 3) Concrete Validation Steps (Fastest First)

### ✅ Step 1 — Recalculate premium manually
- base = 40
- breedRiskFactor = 1.15
- Validate full formula (fees, rounding, taxes)
- Confirm how final premium becomes 52

If math aligns → test expectation is outdated.

---

### ✅ Step 2 — Re-run test with previous risk table version
- Pin RiskTableVersion to previous version (if supported)
- Or mock pricing-service response locally

If result returns ~$45 → confirms expectation drift.

---

### ✅ Step 3 — Review v3.8.0 change log / PR
Check for:
- Labrador factor updates
- Pricing formula adjustments
- Updated golden test values

Confirms whether change was intentional.

---

## 4) What to Include in Bug Report

### 🔎 Summary
PricingFlow test failing due to Labrador premium mismatch after risk table update.

---

### 🧾 Key Evidence
- Test: `PricingFlow.spec.ts › should show premium for Labrador`
- Assertion:

Expected: "$45"
Received: "$52"

- RequestID: `req_7711`
- RiskTableVersion: `2025.10.14-1`
- pricing-service version: `v3.8.0`
- Deployment timestamp: `2025-10-14 09:00:00`
- Failure timestamp: `2025-10-14 09:12:03`

---

### 🕒 Timeline
- 09:00 — pricing-service v3.8.0 deployed
- 09:00 — Risk table updated (Labrador factor changed)
- 09:12 — Test failure observed

---

### 📌 Environment Details
- Environment name (QA/Staging/etc.)
- Whether pricing-service is mocked or live
- Configured RiskTableVersion

---

### ❓ Questions for Product / Actuarial
- Was Labrador factor change intentional?
- Should E2E tests assert exact dollar values?
- Should pricing be version-pinned for deterministic testing?

---

## Conclusion

This appears to be a deterministic pricing change after deployment, not a flaky test or runtime defect. The Playwright test likely contains an outdated expected premium value.

---