# 🧪 Exercise 1: Parse Test Execution Logs

<p align="right">Date: <strong>2026-03-03</strong></p>

---

## 📝 Task

Scenario: A regression test suite just completed with multiple failures. Parse the logs to identify all issues.

**Sample Test Execution Logs:**

```text
2025-10-07 14:30:15 INFO [test-runner] Starting regression suite - 25 tests
2025-10-07 14:30:45 PASS [LoginTest] User authentication successful - 1.2s
2025-10-07 14:31:02 FAIL [CheckoutTest] java.sql.SQLException: Connection refused at line 45
2025-10-07 14:31:15 PASS [SearchTest] Product search returned 8 results - 0.8s
2025-10-07 14:31:30 FAIL [PaymentTest] AssertionError: Expected status 'paid' but got 'pending'
2025-10-07 14:31:45 PASS [CartTest] Add to cart completed - 0.5s
2025-10-07 14:32:00 FAIL [OrderHistoryTest] NullPointerException at OrderService.getHistory(OrderService.java:89)
2025-10-07 14:32:15 FAIL [PaymentRetryTest] AssertionError: Expected retry count 3 but got 0
2025-10-07 14:32:30 PASS [LogoutTest] User logout successful - 0.3s
2025-10-07 14:32:45 FAIL [DatabaseTest] java.sql.SQLException: Deadlock detected in transaction
2025-10-07 14:33:00 INFO [test-runner] Suite completed: 20 passed, 5 failed
```
**Your Task:**

- Paste the logs into ChatGPT
- Use this prompt: "Analyze these test execution logs. Find all test failures, group by error type, and suggest which tests need investigation first."
- Review the AI's classification

---

### 🧾 Result

## Total
- 25 tests run
- 20 passed
- 5 failed

## Failures Grouped by Type

### 1. Database Errors (High Priority)
- CheckoutTest → `SQLException: Connection refused`
- DatabaseTest → `SQLException: Deadlock detected`

### 2. Payment Assertion Failures
- PaymentTest → Expected 'paid', got 'pending'
- PaymentRetryTest → Expected retry count 3, got 0

### 3. Application Exception
- OrderHistoryTest → `NullPointerException` at OrderService.java:89

## Investigation Order

1. **Database infrastructure issues** (may cause cascading failures)
2. **Payment flow logic** (likely related failures)
3. **OrderHistory null handling bug** (isolated code issue)

---