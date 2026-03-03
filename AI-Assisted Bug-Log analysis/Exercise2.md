# 🧪 Exercise 2: Detect Anomalies in Production Logs

<p align="right">Date: <strong>2026-03-03</strong></p>

---

## 📝 Task

Scenario: Your production API has been running normally for weeks, but suddenly you're seeing unusual errors. Use AI to detect anomalies.

**Normal Baseline (Last 30 Days Average):**
```text
Typical error distribution per hour:
- ValidationException: 5-8 errors
- TimeoutException: 2-4 errors
- NullPointerException: 0-1 errors
```

**Today's Production Logs (Last Hour):**
```text
2025-10-07 15:00:12 ERROR [payment-api] ValidationException: Invalid card number
2025-10-07 15:02:45 ERROR [payment-api] ValidationException: Invalid CVV format
2025-10-07 15:05:30 ERROR [payment-api] TimeoutException: Database connection timeout
2025-10-07 15:08:15 ERROR [payment-api] ValidationException: Card expired
2025-10-07 15:10:00 ERROR [payment-api] OutOfMemoryError: Java heap space at PaymentProcessor.java:156
2025-10-07 15:10:05 ERROR [payment-api] OutOfMemoryError: Java heap space at PaymentProcessor.java:156
2025-10-07 15:10:10 ERROR [payment-api] OutOfMemoryError: Java heap space at PaymentProcessor.java:156
2025-10-07 15:12:22 ERROR [payment-api] TimeoutException: Database connection timeout
2025-10-07 15:15:40 ERROR [payment-api] ValidationException: Invalid billing address
2025-10-07 15:18:05 ERROR [payment-api] OutOfMemoryError: Java heap space at PaymentProcessor.java:156
2025-10-07 15:20:30 ERROR [payment-api] TimeoutException: Database connection timeout
2025-10-07 15:25:12 ERROR [payment-api] ValidationException: Payment amount exceeds limit

```
**Your Task:**

- Paste both the baseline and today's logs into ChatGPT
- Ask: "Compare these logs to the baseline. What anomalies do you detect?"
- Ask: "Is this a point anomaly or contextual anomaly?"
- Request: "What should be investigated immediately?"


# 🧾 Result

## Point 1: Paste both the baseline and today's logs into ChatGPT
# Production API Anomaly Report

## Baseline (30-Day Hourly Average)

- ValidationException: 5–8  
- TimeoutException: 2–4  
- NullPointerException: 0–1  
- OutOfMemoryError: Not observed  

---

## Current Hour Observations

- ValidationException: 5 ✅ (Normal)
- TimeoutException: 3 ✅ (Normal)
- NullPointerException: 0 ✅ (Normal)
- OutOfMemoryError: 4 🚨 (Anomalous)

Total Errors: 12

---

## Detected Anomaly

### 🚨 OutOfMemoryError: Java heap space

- New error type (not in baseline)
- 4 occurrences in one hour
- 3 occurred within 10 seconds
- All at `PaymentProcessor.java:156`

This indicates a likely memory leak or uncontrolled memory allocation.

---

## Risk Level: CRITICAL

Potential impact:
- JVM crash
- Service restart
- Production instability

---

## Recommended Actions

1. Check heap usage immediately  
2. Capture heap dump  
3. Review `PaymentProcessor.java:156`  
4. Verify recent deployments or traffic spikes  

---

## Conclusion

A statistically significant anomaly has been detected.  
The sudden appearance and burst pattern of OutOfMemoryError strongly suggests a memory-related defect requiring urgent investigation.

---

## Point 2: Ask: "Compare these logs to the baseline. What anomalies do you detect?"
# Log Anomaly Comparison

## Baseline (Hourly Avg)
- ValidationException: 5–8  
- TimeoutException: 2–4  
- NullPointerException: 0–1  
- OutOfMemoryError: 0  

## Current Hour
- ValidationException: 5 ✅  
- TimeoutException: 3 ✅  
- NullPointerException: 0 ✅  
- OutOfMemoryError: 4 🚨  

## Detected Anomaly

**OutOfMemoryError (Java heap space)**  
- New error type (not in baseline)  
- 4 occurrences in one hour  
- 3 within 10 seconds  
- Same source: `PaymentProcessor.java:156`  

## Conclusion

Critical anomaly detected.  
Likely memory leak or uncontrolled allocation requiring immediate investigation.

---
# Point 3: Ask: "Is this a point anomaly or contextual anomaly?"

## Anomaly Type: Point Anomaly

The detected `OutOfMemoryError` is primarily a **point anomaly**.

### Reasoning:
- The historical baseline shows **0 occurrences**.
- The current hour shows **4 occurrences**.
- Each occurrence individually deviates significantly from normal behavior.

---

## Additional Contextual Pattern

There is also a **contextual characteristic**:

- 3 errors occurred within 10 seconds.
- All originated from `PaymentProcessor.java:156`.

This time-based clustering strengthens the anomaly signal but does not change the primary classification.

---

## Final Classification

**Primary:** Point Anomaly  
**Secondary Characteristic:** Contextual burst behavior

# Point 4: Request: "What should be investigated immediately?"

# Immediate Investigation Actions

## 🚨 Priority: Critical (Memory-Related Failure)

The appearance of `OutOfMemoryError` requires immediate investigation.

## What Should Be Investigated First

### 1. Source Code Location
- Inspect `PaymentProcessor.java:156`
- Look for:
  - Large object creation
  - Unbounded collections (List/Map growth)
  - Infinite loops or recursion
  - Streams without limits
  - Caching without eviction

### 2. JVM Memory Metrics
- Current heap usage
- Garbage collection activity
- Memory growth trend
- Available heap space

### 3. Recent Changes
- New deployments
- Configuration updates
- Dependency/library upgrades
- Traffic pattern changes

### 4. Traffic & Payload Analysis
- Sudden spike in request volume
- Larger-than-normal payload sizes
- Specific request repeatedly triggering failure

---

## Immediate Mitigation (If Memory Is Near Limit)

- Capture heap dump
- Restart affected instance (temporary relief)
- Consider scaling horizontally if traffic spike confirmed