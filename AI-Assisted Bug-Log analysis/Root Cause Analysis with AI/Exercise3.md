# 🧪 Exercise 3: Partner Timeout → Wrong “Product Bug” Assumption (Pet Insurance)

<p align="right">Date: <strong>2026-03-03</strong></p>

---

## 📝 Task

**Goal:** Determine whether a “claim submission failed” is a product defect or an external partner outage (and how the UI/test should behave).

**Steps to implement:**

1. Paste the logs into ChatGPT.
2. Try creating a prompt yourself first (don’t look at the suggested prompt yet).
3. Ask AI to:
  - group errors by component,
  - decide internal vs external root cause,
  - propose immediate mitigation (for test + product),
  - propose what to add to monitoring/tests to prevent confusion next time.
4. Compare your result with the Expected Result.
### 🧾 Result

# Root Cause Analysis – Failed "Submit Claim" Scenario

## 1) Root Cause Classification

**Primary Root Cause: External Dependency Failure**

The failure is caused by degradation of the partner clinic API.

### Evidence:
- `[partner-clinic-api] Timeout calling /clinic/validateInvoice after 10s`
- Monitoring shows p95 latency = **12s** (baseline 500ms)
- Claim marked as `PENDING_EXTERNAL`
- BFF returned `HTTP 202 Accepted`

No deployments occurred for claims-service or BFF, reducing likelihood of internal regression.

---

## 2) Hypotheses Ranked by Likelihood

### 🥇 Hypothesis 1 – External Partner API Degradation (Most Likely)

**Evidence:**
- Explicit timeout error from partner API
- Monitoring shows significant latency spike
- Claim moved to `PENDING_EXTERNAL`
- System responded with `202 Accepted`

**Conclusion:** External SLA breach caused async fallback flow.

---

### 🥈 Hypothesis 2 – Test Expectation Issue

**Evidence:**
- Playwright test expected:

"Claim submitted successfully"

- Backend returned:

202 Accepted
state=PENDING_EXTERNAL


The test assumes synchronous success but system supports async fallback.

**Conclusion:** Test does not handle degraded async scenario.

---

### 🥉 Hypothesis 3 – Timeout Threshold Too Aggressive

**Evidence:**
- Timeout configured at 10s
- Partner p95 latency = 12s

This may indicate timeout needs tuning or retries.

**Conclusion:** Possible resilience tuning issue, but not primary trigger.

---

## 3) Immediate Mitigation Steps

### A. Stabilize CI Tests
- Mock partner-clinic-api in CI
- Stub validation success
- OR update test to support `PENDING_EXTERNAL`

### B. Check Production Health
- Verify current partner API latency
- Check timeout/error rate
- Inspect circuit breaker status
- Check backlog of `PENDING_EXTERNAL` claims

### C. Verify Retry / Recovery Mechanism
- Confirm async retry exists
- Confirm pending claims eventually transition to final state

---

## 4) Recommended UI Communication

Current test expects:
> "Claim submitted successfully"

But backend state is:

PENDING_EXTERNAL


### Recommended UX Messaging:

Instead of:
> Claim submitted successfully

Use:
> Claim received and is being processed.  
> We’re validating your invoice with the clinic.

UI Should:
- Show status badge: `Pending validation`
- Distinguish submission vs final approval
- Optionally auto-refresh or allow manual refresh

---

## 5) Monitoring & Test Improvements

### Improvement #1 – Synthetic Monitoring for Partner API
- Add synthetic check for `/clinic/validateInvoice`
- Alert if:
  - p95 > 2s
  - Timeout rate > 5%

---

### Improvement #2 – Add Async Flow Test Case

Add test scenario:

When partner validation times out
Then claim state = PENDING_EXTERNAL
And user sees "Claim received and processing"


---

### Bonus Improvement – Observability Correlation

Dashboard correlation between:
- Partner API latency
- Timeout rate
- Spike in `PENDING_EXTERNAL`
- UI test failures

---

# Final Conclusion

| Item | Conclusion |
|------|------------|
| Root Cause | External dependency degradation |
| Internal Bug | No |
| Test Issue | Yes – assumes synchronous success |
| Production Risk | Medium – claims enter async state |

---