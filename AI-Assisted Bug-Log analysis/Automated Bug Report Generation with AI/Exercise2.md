# 🧪 Exercise 2: Convert Manual Bug to AI-Generated Report

<p align="right">Date: <strong>2026-03-03</strong></p>

---

## 📝 Task

Scenario: You found a bug manually during exploratory testing

**Your Notes:**

```text
- Testing the checkout flow
- Mobile app version 2.3.1 on iOS 16
- Clicked "Apply Coupon" button
- App crashed
- Restarted app, tried again - same crash
- Checked with Android - works fine
- Only happens with coupons that expired yesterday
- Console shows: "Cannot read property 'validUntil' of undefined"
```
**Your Task:**

1. Convert your notes into structured log format
2. Ask ChatGPT to generate proper bug ticket
3. Ensure it includes: browser/device info, repro steps, error details
4. Add: Expected behavior (what should happen with expired coupons?)


### 🧾 Result

# 1. Convert your notes into structured log format

# 🐞 Bug Report – App Crash on Applying Expired Coupon (iOS)

## 1. Summary
The mobile app crashes when attempting to apply a coupon that expired yesterday on iOS. The issue does not occur on Android.

---

## 2. Environment
- **Platform:** iOS  
- **OS Version:** iOS 16  
- **App Version:** 2.3.1  
- **Device Type:** Mobile  
- **Testing Type:** Exploratory Testing  
- **Tester Role:** Senior QA Engineer  

---

## 3. Test Area
- **Feature:** Checkout Flow  
- **Action:** Apply Coupon  

---

## 4. Steps to Reproduce
1. Open the mobile app (v2.3.1) on iOS 16.
2. Navigate to the checkout flow.
3. Tap the **"Apply Coupon"** button.
4. Enter/select a coupon that expired yesterday.
5. Confirm coupon application.

---

## 5. Expected Result
The app should display a validation message indicating that the coupon is expired and prevent application without crashing.

---

## 6. Actual Result
The app crashes immediately after attempting to apply the expired coupon.

---

## 7. Reproducibility
- **Frequency:** 100%  
- **Retested:** Yes (after app restart)  
- **Result After Restart:** Same crash occurs  

---

## 8. Cross-Platform Check
- **Android:** Works as expected (no crash; proper handling of expired coupon)  
- **iOS:** Crashes consistently  

---

## 9. Console/Error Logs


---

# 2. Ask ChatGPT to generate proper bug ticket
# 3. Ensure it includes: browser/device info, repro steps, error details
# 4. Add: Expected behavior (what should happen with expired coupons?)
# Result of point 2-4

# 🐞 Bug Ticket

## Title
[iOS 16][v2.3.1] App crashes when applying coupon expired yesterday – `Cannot read property 'validUntil' of undefined`

---

## Issue Type
Bug

## Severity
Critical – Application Crash

## Priority
High

---

## Environment Details

### Device Information
- **Platform:** iOS  
- **OS Version:** iOS 16  
- **App Version:** 2.3.1  
- **Device Type:** Mobile (iPhone – exact model TBD)  
- **Build Type:** Production/TestFlight (confirm if needed)  

### Browser / Runtime Information
- **Rendering Engine:** iOS WebView (WKWebView – assumed)  
- **JavaScript Runtime:** iOS JS Engine  
- **Console Access:** Xcode / Safari Web Inspector  

### Cross-Platform Check
- **Android (same scenario):** ✅ Works correctly (no crash, proper expired coupon handling)  
- **iOS:** ❌ Crashes consistently  

---

## Affected Module
Checkout Flow → Coupon Application / Validation Logic

---

## Description
During exploratory testing of the checkout flow, the application crashes when attempting to apply a coupon that expired yesterday. The issue is isolated to iOS devices running app version 2.3.1.

The crash occurs immediately after tapping the **"Apply Coupon"** button when the coupon is expired (specifically those that expired yesterday).

The issue persists after restarting the application and is reproducible 100% of the time.

---

## Preconditions
- User has items in cart.
- User is on Checkout screen.
- A coupon exists in the system that expired yesterday.

---

## Steps to Reproduce

1. Open the mobile app (v2.3.1) on an iOS 16 device.  
2. Navigate to the Checkout flow.  
3. Tap the **"Apply Coupon"** button.  
4. Enter or select a coupon that expired yesterday.  
5. Confirm the coupon application.  

---

## Expected Behavior

When a user attempts to apply an expired coupon:

- The system should validate the coupon expiration date.
- The app should display a clear validation message (e.g., **"This coupon has expired"**).
- The coupon should not be applied to the order.
- The checkout flow should remain accessible.
- The application must remain stable without crashing.
- Error handling should gracefully manage undefined or invalid coupon objects.
- The behavior should be consistent across platforms (same as Android implementation).

---

## Actual Result

- The app crashes immediately after attempting to apply the expired coupon.
- No validation message is displayed.
- User is returned to the OS home screen.

---

## Error Details

### Console Error

Cannot read property 'validUntil' of undefined


### Observations
- Indicates an attempt to access `validUntil` on an undefined object.
- Suggests missing null/undefined validation in coupon response handling.
- Likely occurs during expiration validation logic.

---

## Reproducibility
- 100% reproducible  
- Occurs after app restart  
- Only reproducible on iOS  
- Specific to coupons that expired yesterday  

---

## Impact
- Causes full app crash during checkout.
- Blocks user from completing purchase if expired coupon is attempted.
- High risk to production stability and checkout conversion.

---

## Suspected Root Cause
Coupon object is undefined when validating expiration date on iOS implementation. Missing defensive check before accessing `validUntil` property.

---

## Recommended Next Steps
- Add null/undefined checks before accessing coupon properties.
- Align iOS validation logic with Android implementation.
- Add unit and edge-case tests for:
  - Recently expired coupons  
  - Null/undefined coupon response  
  - Invalid coupon payload  
- Verify backend response structure for expired coupon scenario.

---

## Attachments (To Be Added)
- Screen recording  
- Crash log  
- Full console trace  
- Test coupon ID used for reproduction  