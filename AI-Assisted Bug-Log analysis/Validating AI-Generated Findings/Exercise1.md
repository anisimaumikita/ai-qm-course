# 🧪 Exercise 1: Spot the Hallucination

<p align="right">Date: <strong>2026-03-03</strong></p>

---

## 📝 Task

Scenario: AI analyzed these logs and provided findings. Your job: verify accuracy.

**Original Logs:**

```text
2025-10-06 15:30:12 ERROR [api-gateway]
Request timeout after 5000ms
Endpoint: GET /api/users/profile
RequestID: req_xyz789
2025-10-06 15:30:15 ERROR [api-gateway]
Request timeout after 5000ms
Endpoint: GET /api/users/settings
RequestID: req_abc456

```

**AI Analysis:**

```text
1. Total errors: 2
2. Error type: Request timeout
3. Timeout value: 5000ms (5 seconds)
4. Affected endpoints: /api/users/profile, /api/users/settings
5. Connection pool size: 20 connections
6. Database query causing timeout: "SELECT * FROM users WHERE id = ?"
7. Started: 15:30:12
8. Duration: 3 seconds
9. Root cause: Database performance degradation
10. Users affected: 47

```

**Your Task:**

1. Mark each AI claim as: Accurate, Hallucination, or Assumption
2. For hallucinations: Identify what's invented
3. For assumptions: Identify what needs verification


### 🧾 Result


---

# 2️⃣ Extracted Verified Facts

| Field | Verified Value |
|--------|---------------|
| Total ERROR entries | 2 |
| Service | api-gateway |
| Error type | Request timeout |
| Timeout value | 5000ms |
| First timestamp | 15:30:12 |
| Last timestamp | 15:30:15 |
| Time span between errors | 3 seconds |
| Affected endpoints | /api/users/profile, /api/users/settings |
| Unique RequestIDs | 2 |

---

# 3️⃣ Claim-by-Claim Validation

| # | AI Claim | Verdict | Validation Result |
|---|----------|----------|-------------------|
| 1 | Total errors: 2 | ✅ Accurate | Matches exact count of ERROR entries |
| 2 | Error type: Request timeout | ✅ Accurate | Explicitly stated in logs |
| 3 | Timeout value: 5000ms (5 seconds) | ✅ Accurate | Directly present in logs |
| 4 | Affected endpoints | ✅ Accurate | Both endpoints appear in logs |
| 5 | Connection pool size: 20 | ❌ Hallucination | No pool size or connection data present |
| 6 | SQL query: `SELECT * FROM users WHERE id = ?` | ❌ Hallucination | No SQL or database logs shown |
| 7 | Started: 15:30:12 | ✅ Accurate | Earliest timestamp in dataset |
| 8 | Duration: 3 seconds | ✅ Accurate | 15:30:15 − 15:30:12 = 3 seconds |
| 9 | Root cause: Database performance degradation | ⚠ Assumption | No causal evidence in logs |
|10 | Users affected: 47 | ❌ Hallucination | No user metrics or counts available |

---

# 4️⃣ Hallucination Breakdown

## Claim #5 — Connection Pool Size
**Invented Elements:**
- Pool concept
- Pool size = 20
- Any database configuration detail

No such data exists in the logs.

---

## Claim #6 — Specific SQL Query
**Invented Elements:**
- Exact SQL statement
- Assumed database layer involvement

No stack trace or query logs support this.

---

## Claim #10 — 47 Users Affected
**Invented Elements:**
- User count
- Any aggregation logic
- Any analytics source

Only 2 RequestIDs exist in the dataset.

---

# 5️⃣ Assumption Breakdown

## Claim #9 — Root Cause: Database Performance Degradation

This is not directly supported by the logs.

Possible alternative causes (not verifiable from dataset):

- Downstream service latency
- Network issue
- Thread pool saturation
- External dependency delay

No database errors, slow query logs, or stack traces appear.

This remains **unverified causation**.

---

# 6️⃣ Validation Metrics

| Metric | Value |
|--------|--------|
| Accurate Claims | 6 |
| Assumptions | 1 |
| Hallucinations | 3 |
| Accuracy Rate | 60% |
| Hallucination Rate | 30% |

---

# 7️⃣ Reliability Assessment

### Positive Indicators
- Correct extraction of visible log data
- Correct timestamp calculation
- Accurate timeout interpretation

### Reliability Red Flags
- Fabricated infrastructure details
- Invented database query
- Fabricated user impact count
- Unsupported root cause conclusion

**Overall Reliability: Moderate**

AI correctly extracts surface-level data but introduces speculative technical details without evidence.

---

# 8️⃣ Completeness Assessment

### What AI Identified Correctly
- Error type
- Timeout value
- Affected endpoints
- Error count
- Time window

### What AI Failed to Do
- Explicitly state service name (api-gateway)
- Highlight unique RequestIDs
- Recommend cross-service trace
- Indicate need for deeper validation before root cause assignment

Completeness is acceptable for surface analysis but insufficient for incident-level reporting.

---

# 9️⃣ Correct Validated Conclusion

Based solely on the provided logs:

- Two request timeouts occurred in `api-gateway`
- Both timed out after 5000ms
- Two different endpoints were affected
- Two distinct requests were involved
- Errors occurred between 15:30:12 and 15:30:15 (3-second span)

Root cause cannot be determined from the provided dataset alone.

---

# 🔟 Final Assessment

The AI demonstrates:

- ✅ Strong log parsing ability  
- ❌ Moderate hallucination risk  
- ⚠ Overconfident root cause inference  

**Key Lesson:**  
AI output should be treated as a hypothesis.  
Only directly observable log data should be reported as confirmed facts.

---