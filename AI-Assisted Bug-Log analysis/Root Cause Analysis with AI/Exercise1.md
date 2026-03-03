# 🧪 Exercise 1: Correlate Bug with Test Cases

<p align="right">Date: <strong>2026-03-03</strong></p>

---

## 📝 Task

Scenario: User login fails on mobile app with special characters in password

**Production Logs:**

```text
2025-10-06 14:23:45.123 ERROR [auth-service]
Authentication failed: Invalid password encoding
User: john.doe@example.com
Password validation failed at PasswordEncoder.encode()
Stack trace:
  at PasswordEncoder.encode(PasswordEncoder.java:42)
  at AuthService.authenticate(AuthService.java:89)
Platform: Mobile App v2.1.0
User-Agent: Mobile/iOS/2.1.0
Request payload: {"username":"john.doe@example.com","password":"P@ssw0rd!#"}
Error: 401 Unauthorized - Special characters not properly URL-encoded

```
**Bug Pattern Analysis:**

- Only occurs on Mobile app v2.1.0 (Web app v3.2.0 works fine)
- Affects passwords with special characters: !@#$%^&*()
- Started after mobile app refactoring in v2.1.0
- 50+ users reported login failures in 2 days
---
**Existing Test Cases:**

| Test ID        | Test Case                        | Password Used      | Result |
|----------------|----------------------------------|--------------------|--------|
| TC-LOGIN-001   | Valid username and password      | "Password123"      | ✓ Pass |
| TC-LOGIN-002   | Invalid password                 | "wrongpass"        | ✓ Pass |
| TC-LOGIN-003   | Empty username/password          | ""                 | ✓ Pass |
| TC-LOGIN-004   | Password with spaces             | "Pass Word 123"    | ✓ Pass |

**Requirement:**

```text
REQ-AUTH-101: User Authentication
Users must be able to login with username and password
Password requirements:
- 8-20 characters
- At least 1 uppercase letter
- At least 1 number
- Special characters allowed: !@#$%^&*()
Note: No mention of encoding requirements or platform differences
```
**Your Task:**

1. Paste the logs, test cases, and requirement to ChatGPT
2. Ask: "What test case is missing that should have caught this bug?"
3. Request: "Suggest new test cases to prevent this in the future"
4. Ask: "Is this a requirement gap, test coverage gap, or implementation issue?"
5. Ask: "Which test case would have caught this if properly designed?"

### 🧾 Result

# 1. Paste the logs, test cases, and requirement to ChatGPT

## Root Cause Analysis

## 🔎 Log Analysis Summary

### Observations
- Error: **401 Unauthorized – Special characters not properly URL-encoded**
- Failure location: `PasswordEncoder.encode()`
- Affected platform: **Mobile App v2.1.0**
- Web App v3.2.0 works correctly
- Issue occurs only when passwords contain special characters: `!@#$%^&*()`
- Issue started after Mobile App refactoring in v2.1.0
- 50+ users impacted within 2 days

---

## 📌 Identified Pattern

- Only Mobile platform affected
- Only passwords with special characters fail
- Backend authentication logic unchanged
- Failure triggered before password validation comparison
- Error explicitly mentions encoding issue

---

## 🎯 Root Cause

The **Mobile App v2.1.0** is not properly URL-encoding or escaping special characters in the password field before sending the authentication request.

As a result:

1. Special characters are altered or misinterpreted during transmission.
2. Backend receives a corrupted password string.
3. `PasswordEncoder.encode()` processes an incorrect value.
4. Password comparison fails.
5. System returns **401 Unauthorized**.

---

## 🧩 Conclusion

This is a **client-side encoding defect introduced during Mobile App refactoring (v2.1.0)**.

It is **not**:
- A backend authentication logic issue
- A password policy validation issue
- A requirement defect

It is specifically a **mobile request encoding implementation issue**.

# Point 2: Ask: "What test case is missing that should have caught this bug?"

# Missing Test Case That Should Have Caught This Bug

## 🎯 Missing Test Case

### Test ID: TC-LOGIN-005  
### Test Name: Login with Password Containing Allowed Special Characters (Mobile)

---

## 📋 Objective
Verify that users can successfully log in using passwords that contain allowed special characters (`!@#$%^&*()`) on the **Mobile App**.

---

## 🧪 Preconditions
- User account exists.
- Password meets REQ-AUTH-101 requirements:
  - 8–20 characters
  - At least 1 uppercase letter
  - At least 1 number
  - Contains allowed special character(s)

Example Password: `P@ssw0rd!#`

---

## 🔄 Test Steps

1. Open Mobile App v2.1.0.
2. Navigate to Login screen.
3. Enter valid username.
4. Enter password containing special characters.
5. Tap **Login**.

---

## ✅ Expected Result
- Authentication request is properly encoded.
- Backend receives correct password value.
- User is successfully authenticated.
- User is redirected to the dashboard/home screen.
- No 401 Unauthorized error.

---

## ❌ Actual Result (Production)
- 401 Unauthorized returned.
- Authentication fails.
- Error: "Special characters not properly URL-encoded"

---

## 📌 Why This Test Case Was Critical

- Special characters are explicitly allowed in REQ-AUTH-101.
- No existing test validates special characters on the Mobile platform.
- Existing test cases only cover:
  - Alphanumeric passwords
  - Invalid passwords
  - Empty fields
  - Spaces in password
- No cross-platform parity validation between Web and Mobile.

---

## 🧩 Conclusion

The missing test case is:

> **Login with valid password containing allowed special characters on Mobile platform.**

If this test had been included in regression testing after the Mobile v2.1.0 refactor, the defect would have been detected before production release.

# Point 3: Request: "Suggest new test cases to prevent this in the future"
Suggested New Test Cases to Prevent Future Occurrence

To prevent similar encoding-related authentication issues, the following test cases should be added to the regression and mobile test suite.

---

## 🧪 A. Functional Test Cases (Special Characters Coverage)

| Test ID | Test Case Description | Password Example | Platform | Expected Result |
|----------|----------------------|------------------|------------|----------------|
| TC-LOGIN-011 | Password with single allowed special character | `Password1!` | Mobile | Login Successful |
| TC-LOGIN-012 | Password with multiple allowed special characters | `P@ssw0rd!#` | Mobile | Login Successful |
| TC-LOGIN-013 | Password containing all allowed special characters | `Aa1!@#$%^&*()` | Mobile | Login Successful |
| TC-LOGIN-014 | Special character at beginning | `!Password1` | Mobile | Login Successful |
| TC-LOGIN-015 | Special character at end | `Password1!` | Mobile | Login Successful |
| TC-LOGIN-016 | Special characters mixed with spaces | `Pass word1!` | Mobile | Login Successful |
| TC-LOGIN-017 | Boundary length (8 chars) with special character | `Aa1!aaaa` | Mobile | Login Successful |
| TC-LOGIN-018 | Boundary length (20 chars) with special character | `Aa1!aaaaaaaaaaaaaaa` | Mobile | Login Successful |

---

## 🔍 B. Negative & Encoding Edge Case Tests

| Test ID | Test Case Description | Password Example | Expected Result |
|----------|----------------------|------------------|----------------|
| TC-LOGIN-019 | Password containing URL-sensitive characters | `Pass%word1!` | Login Successful |
| TC-LOGIN-020 | Password containing plus sign | `Pass+word1!` | Login Successful |
| TC-LOGIN-021 | Password containing ampersand | `Pass&word1!` | Login Successful |
| TC-LOGIN-022 | Password containing parentheses | `Pass(word)1!` | Login Successful |
| TC-LOGIN-023 | Verify raw request payload encoding | Any special char password | Password received correctly by backend |

---

## 🔄 C. Cross-Platform Parity Tests

| Test ID | Test Case Description | Platform Comparison | Expected Result |
|----------|----------------------|--------------------|----------------|
| TC-LOGIN-024 | Same credentials tested on Web & Mobile | Web vs Mobile | Identical authentication result |
| TC-LOGIN-025 | Automated regression suite execution after refactor | All platforms | No authentication discrepancies |

---

## 🔌 D. API-Level Validation Tests

1. Capture outgoing request payload from Mobile app.
2. Verify password field is properly JSON encoded.
3. Confirm backend receives exact string value.
4. Compare encoded vs decoded values.
5. Validate HTTP headers:
   - `Content-Type: application/json`

---

## 🤖 E. Automation Recommendations

- Add authentication regression tests to CI/CD pipeline.
- Include special character test data set.
- Run automated cross-platform authentication tests after every build.
- Add API contract tests to validate request formatting.

---

## 📌 Preventive Strategy Summary

To prevent recurrence:

- Expand special character coverage.
- Add mobile-specific regression testing.
- Implement cross-platform parity validation.
- Validate encoding at API level.
- Make encoding validation part of Definition of Done.

---

## 🎯 Outcome

By introducing these test cases, encoding defects caused by refactoring or platform-specific implementation changes will be detected before production deployment.

# Point 4: Ask: "Is this a requirement gap, test coverage gap, or implementation issue?"

Classification of the Issue

## 🎯 Final Classification: **Implementation Issue (Primary)**
With a **Test Coverage Gap (Secondary)**  
Not a Requirement Gap.

---

## 📌 1️⃣ Requirement Analysis

**REQ-AUTH-101 states:**
- Password length: 8–20 characters
- At least 1 uppercase letter
- At least 1 number
- Special characters allowed: `!@#$%^&*()`
- Users must be able to log in with username and password

There is:
- ✅ Clear allowance of special characters
- ✅ No conflicting rules
- ✅ No platform-specific restriction

**Conclusion:**  
The requirement is clear and correct.  
There is no ambiguity or missing functional rule.

➡️ **Not a Requirement Gap**

---

## 📌 2️⃣ Technical Behavior Observed

- Web app works correctly.
- Mobile App v2.1.0 fails.
- Error indicates improper URL encoding.
- Backend authentication logic unchanged.
- Issue introduced after mobile refactoring.

This indicates:
- Mobile client is not properly encoding special characters.
- Authentication logic itself is functioning as designed.

➡️ **Primary Root Cause: Implementation Defect (Mobile Client)**

---

## 📌 3️⃣ Test Coverage Assessment

Existing test cases do NOT include:
- Special character password validation on Mobile
- Cross-platform authentication parity testing
- API payload encoding verification
- Regression tests post-refactor for authentication edge cases

If such tests had existed, this defect would likely have been caught before release.

➡️ **Secondary Cause: Test Coverage Gap**

---

## 🧩 Final Conclusion

| Category | Status | Explanation |
|------------|------------|-------------|
| Requirement Gap | ❌ No | Requirement clearly allows special characters |
| Test Coverage Gap | ⚠️ Yes (Contributing Factor) | Missing special character and mobile parity tests |
| Implementation Issue | ✅ Yes (Primary Cause) | Mobile app failed to properly encode password during refactor |

---

## 🏁 Final Answer

This is primarily an **Implementation Issue**, with a contributing **Test Coverage Gap**, and **not a Requirement Gap**.

# Point 5: Ask: "Which test case would have caught this if properly designed?"

# Which Test Case Would Have Caught This If Properly Designed?

## 🎯 The Test Case That Should Have Caught It

### TC-LOGIN-005  
**Test Name:** Login with Valid Password Containing Allowed Special Characters (Mobile Platform)

---

## 📋 Properly Designed Version of the Test Case

### Objective  
Verify that the Mobile application correctly handles and transmits passwords containing allowed special characters during authentication.

---

### Preconditions  
- User account exists.
- Password meets REQ-AUTH-101:
  - 8–20 characters
  - At least 1 uppercase letter
  - At least 1 number
  - Contains allowed special character(s)

Example Password:  
`P@ssw0rd!#`

---

### Test Steps  

1. Launch Mobile App (target build).
2. Enter valid username.
3. Enter password containing special characters.
4. Submit login request.
5. Capture outgoing request payload (via proxy/logs).
6. Verify backend receives exact password string.
7. Verify authentication result.

---

### Expected Results  

- Password is properly JSON/URL encoded before transmission.
- Backend receives exact original password value.
- `PasswordEncoder.encode()` processes correct input.
- User successfully logs in.
- No 401 Unauthorized error.

---

## 🔎 Why This Test Would Have Caught the Bug

If properly designed, it would:

- Be executed specifically on the **Mobile platform**
- Include **special character combinations**
- Validate **actual request payload encoding**
- Compare Web vs Mobile behavior
- Be included in regression after refactoring

The existing tests:
- Only validated functionality
- Did not validate encoding behavior
- Did not ensure cross-platform consistency

---

## 🧩 Key Insight

A simple positive login test with special characters on Mobile —  
**combined with payload validation** — would have detected this defect before production.

---

## 🏁 Final Answer

The test case that would have caught this is:

> **A Mobile-specific login test using a valid password containing allowed special characters, with proper request payload encoding validation.**

---