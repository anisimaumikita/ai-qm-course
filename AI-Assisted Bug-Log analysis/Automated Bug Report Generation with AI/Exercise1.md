# 🧪 Exercise 1: Generate Bug from Production Incident

<p align="right">Date: <strong>2026-03-03</strong></p>

---

## 📝 Task

Scenario: Database connection pool exhaustion in production

**Production Logs:**

```text
2025-10-06 17:00:00.789 ERROR [user-service]
Failed to acquire database connection
java.sql.SQLTimeoutException: Timeout waiting for connection from pool
  at com.zaxxer.hikari.pool.HikariPool.getConnection(HikariPool.java:197)
  at com.example.user.repository.UserRepository.findById(UserRepository.java:45)
  at com.example.user.service.UserService.getUserById(UserService.java:78)
RequestID=req_abc123 UserID=12346 Timeout=30s PoolSize=20 ActiveConnections=20
2025-10-06 17:00:05.123 WARN [hikari-pool]
Found 15 connections open for >5 minutes
Active queries:
- Query 1: "SELECT * FROM user_activity WHERE created_at > ?" (runtime: 8m 23s)
- Query 2: "SELECT * FROM user_sessions WHERE..." (runtime: 6m 45s)
2025-10-06 17:00:10.456 ERROR [monitoring]
Connection leak suspected in UserActivityService
Total errors: 45 in 5 minutes
Services affected: user-service, notification-service, reporting-service
```
**Your Task:**

1. Use ChatGPT to generate bug ticket structure (title, steps, root cause, stack trace)
2. Ask AI: "What are the technical symptoms and likely root cause?"
3. Ask AI: "Is this infrastructure issue or code bug? Explain evidence."
4. Ask AI: "Suggest immediate mitigation and long-term fix"


### 🧾 Result

# 1. Use ChatGPT to generate bug ticket structure (title, steps, root cause, stack trace)

# 🐞 Bug Ticket: Database Connection Pool Exhaustion in Production

## Title
Production Incident – Database Connection Pool Exhaustion Causing Service Failures

---

## Environment
- Environment: Production
- Services Affected: user-service, notification-service, reporting-service
- Connection Pool: HikariCP
- Pool Size: 20
- Timeout Threshold: 30 seconds

---

## Description
Multiple services are failing to acquire database connections due to exhaustion of the HikariCP connection pool. All 20 connections are active, with several connections running long-running queries (>5 minutes). Monitoring reports a suspected connection leak in `UserActivityService`.

---

## Steps to Reproduce
1. Deploy application with HikariCP pool size set to 20.
2. Trigger heavy database activity (especially queries on `user_activity` and `user_sessions`).
3. Allow long-running queries to execute without proper timeout handling.
4. Observe connection pool saturation.
5. Attempt additional requests requiring DB access.
6. System throws `SQLTimeoutException` after 30 seconds waiting for a connection.

---

## Expected Result
- Database connections should be released promptly after query execution.
- Long-running queries should be optimized or time-limited.
- Services should continue handling requests without connection acquisition failures.

---

## Actual Result
- All 20 connections remain active.
- 15 connections open for more than 5 minutes.
- New requests fail with timeout while waiting for available connection.
- 45 errors recorded within 5 minutes.
- Multiple services impacted.

---

## Root Cause Analysis
Preliminary findings indicate:

1. **Long-Running Queries**
   - Query on `user_activity` running for 8m 23s.
   - Query on `user_sessions` running for 6m 45s.
   - Possible missing indexes or inefficient query patterns.

2. **Connection Leak Suspected**
   - Monitoring flagged `UserActivityService`.
   - Connections may not be properly closed after execution.
   - Potential missing `try-with-resources` or improper transaction handling.

3. **Pool Configuration Constraints**
   - Pool size limited to 20.
   - No query timeout enforcement.
   - No connection leak detection threshold configured.

---

## Stack Trace

# 2. Ask AI: "What are the technical symptoms and likely root cause?"

# 🔎 Technical Symptoms and Likely Root Cause

## Technical Symptoms

### 1. Connection Pool Exhaustion
- `SQLTimeoutException: Timeout waiting for connection from pool`
- PoolSize=20
- ActiveConnections=20 (100% utilization)
- Requests failing after 30s timeout

### 2. Long-Running Queries
- 15 connections open for >5 minutes
- Example queries:
  - `SELECT * FROM user_activity WHERE created_at > ?` (8m 23s)
  - `SELECT * FROM user_sessions WHERE ...` (6m 45s)
- Indicates poor query performance or missing indexing

### 3. Elevated Error Rate
- 45 errors in 5 minutes
- Services impacted:
  - user-service
  - notification-service
  - reporting-service
- Cascading service degradation

### 4. Suspected Connection Leak
- Monitoring alert: “Connection leak suspected in UserActivityService”
- Connections possibly not being closed or returned to pool
- Potential improper transaction management or missing resource cleanup

### 5. Service-Level Symptoms
- Increased request latency
- API timeouts
- Partial or complete service unavailability
- Thread blocking while waiting for DB connection

---

# 🧠 Likely Root Cause

## Primary Root Cause

### Combination of Long-Running Queries + Connection Leak

1. Long-running queries are holding DB connections for extended durations.
2. Connections are not being released promptly (possible leak).
3. Pool size (20) becomes fully consumed.
4. New requests block until timeout.
5. Timeout exceptions propagate across services.

---

## Contributing Factors

### 1. Inefficient Queries
- Full table scans (e.g., `SELECT *`)
- Missing indexes on `created_at`
- No pagination or result size limits

### 2. Improper Resource Handling
- Missing `try-with-resources`
- Connections not closed in exception paths
- Long-lived transactions

### 3. Pool Configuration Gaps
- No `leakDetectionThreshold` configured
- No query timeout enforcement
- Pool size not aligned with traffic load

### 4. Lack of Backpressure
- Services continue accepting requests despite pool saturation
- No circuit breaker or rate limiting

---

# 📌 Conclusion

The incident is most likely caused by a **connection leak combined with long-running, unoptimized queries**, resulting in full exhaustion of the HikariCP connection pool and cascading failures across dependent services.

# 3. Ask AI: "Is this infrastructure issue or code bug? Explain evidence."

# 🧩 Is This an Infrastructure Issue or a Code Bug?

## ✅ Conclusion
This is **primarily a code-level issue**, not an infrastructure failure.

It is caused by application behavior (connection handling + inefficient queries), not by database server downtime, network failure, or hardware resource exhaustion.

---

# 🔎 Evidence Analysis

## 1️⃣ Evidence of Connection Pool Saturation (Application-Level)

From logs:


PoolSize=20
ActiveConnections=20
Timeout=30s
java.sql.SQLTimeoutException: Timeout waiting for connection from pool


### What This Means
- The connection pool is fully utilized (100%).
- The application cannot obtain a free connection within 30 seconds.
- This error originates from **HikariCP inside the application**, not from the database server.

If this were infrastructure:
- We would see connection refused errors.
- Or database unreachable exceptions.
- Or network/socket-level failures.

Instead, we see **application-level pool exhaustion**.

---

## 2️⃣ Evidence of Long-Running Queries


Found 15 connections open for >5 minutes
Query runtime: 8m 23s
Query runtime: 6m 45s


### What This Indicates
- Connections are alive but busy.
- Queries are executing for several minutes.
- Likely missing indexes or inefficient query design.
- Possibly large result sets (`SELECT *`).

This strongly suggests **query optimization issue**, which is code/schema related — not infrastructure.

---

## 3️⃣ Evidence of Connection Leak


Connection leak suspected in UserActivityService


This points directly to:

- Improper connection handling in application code.
- Connections not being closed properly.
- Long transactions not being committed/rolled back.

Infrastructure problems do NOT cause connection leaks.
Only application code can fail to release connections.

---

## 4️⃣ No Signs of Infrastructure Failure

Missing indicators of infra issues:

❌ No "connection refused"  
❌ No "database unavailable"  
❌ No "network timeout"  
❌ No CPU/memory crash logs  
❌ No database restart logs  

Connections are successfully established — they just aren't being released.

---

# 🏗 If It Were Infrastructure, We Would See:

| Infra Problem | Expected Symptom |
|--------------|-----------------|
| DB down | `Cannot connect to database` |
| Network issue | `SocketTimeoutException` |
| CPU/memory exhaustion | OOM, DB crash logs |
| Disk I/O bottleneck | DB-level performance alerts |

None of these appear in the logs.

---

# 🎯 Final Determination

## Primary Cause: **Code Bug**
- Connection leak in `UserActivityService`
- Long-running, unoptimized queries
- Possibly missing transaction boundaries

## Secondary Contributor: **Configuration Gap**
- Small pool size (20)
- No leak detection threshold
- No query timeout enforcement

But configuration alone would not cause 8-minute queries or leaked connections.

---

# 🧠 Final Answer

This incident is **not an infrastructure issue**.  
It is a **code-level defect combined with poor query optimization**, leading to connection pool exhaustion and cascading service failures.

# 4. Ask AI: "Suggest immediate mitigation and long-term fix"

# 🚑 Immediate Mitigation & 🛠 Long-Term Fix Plan

## 🚑 Immediate Mitigation (Stabilize Production)

### 1️⃣ Restart Affected Services
- Restart `user-service`, `notification-service`, and `reporting-service`.
- This immediately releases stuck/leaked connections.
- Restores pool availability temporarily.

---

### 2️⃣ Kill Long-Running Queries
From the database side:
- Identify queries running > 2–3 minutes.
- Terminate:
  - `SELECT * FROM user_activity WHERE created_at > ?`
  - `SELECT * FROM user_sessions WHERE ...`
- Frees up active DB connections immediately.

---

### 3️⃣ Temporarily Increase Pool Size
If DB capacity allows:
- Increase HikariCP pool size (e.g., 20 → 40).
- Increase connection timeout slightly (short-term only).
  
⚠️ This is a temporary workaround, not a real fix.

---

### 4️⃣ Enable Leak Detection
Configure:


leakDetectionThreshold=20000


- Logs stack trace of connections not returned within 20s.
- Helps identify exact code location.

---

### 5️⃣ Apply Rate Limiting / Traffic Control
- Reduce incoming load temporarily.
- Enable circuit breaker to prevent cascading failures.
- Protect DB from overload.

---

### 6️⃣ Add Query Timeout
Example:


spring.jpa.properties.hibernate.jdbc.timeout=30


- Prevents queries from running 8+ minutes.

---

# 🛠 Long-Term Fix (Permanent Resolution)

## 1️⃣ Fix Connection Leak in Code (Highest Priority)

### Ensure Proper Resource Handling

Use `try-with-resources`:

```java
try (Connection conn = dataSource.getConnection();
     PreparedStatement stmt = conn.prepareStatement(sql)) {
    ...
}

OR ensure:

Transactions are properly committed/rolled back.

Connections are closed in finally blocks.

No async threads holding connections.

2️⃣ Optimize Long-Running Queries
Improve user_activity Query

Add index on created_at

Avoid SELECT *

Add pagination (LIMIT / OFFSET)

Analyze execution plan

Improve user_sessions

Add proper filtering index

Remove unnecessary joins

Reduce result set size

3️⃣ Implement Query Performance Monitoring

Enable slow query log in DB.

Alert when query > 5 seconds.

Integrate APM (Datadog, New Relic, etc.).

4️⃣ Right-Size Connection Pool

Instead of arbitrary pool size:

Pool Size ≈ (CPU cores × 2) + effective disk capacity

Validate DB max connections

Align pool sizes across microservices

5️⃣ Configure Proper Timeouts

Recommended production configuration:

maximumPoolSize=30
minimumIdle=10
connectionTimeout=30000
idleTimeout=600000
maxLifetime=1800000
leakDetectionThreshold=20000
6️⃣ Add Resilience Patterns

Circuit breaker

Bulkhead isolation

Timeout at service layer

Backpressure handling

7️⃣ Improve Observability

Track:

Active vs Idle connections

Query duration percentiles

Connection wait time

Pool exhaustion events

Add alert:

If ActiveConnections > 80% for 2 minutes → trigger alert

📌 Summary
Immediate Goal

Restore service quickly by:

Restarting services

Killing long-running queries

Temporarily increasing pool size

Permanent Fix

Eliminate:

Connection leak

Inefficient queries

Missing timeout configuration

🎯 Final Takeaway

This incident is preventable.
The long-term solution is:

Fix the code + Optimize queries + Add proper observability + Enforce timeouts

Not just increasing the pool size.

---