# QM Review – Smart Home Lighting Schedule Requirement

## 1. Initial AI-Generated Checklist (Review Input)

- Schedule lights to turn on → success  
- Schedule lights to turn off → success  

### Evaluation
The checklist is **too simplistic** and does not cover important functional, edge, or usability scenarios. It validates only the most basic successful operations.

---

# 2. Missing Scenarios Identified

At least the following important cases are missing:

### Functional Edge Cases
1. **Overlapping schedules**
   - Example: Lights scheduled ON at 18:00 and OFF at 18:00 or conflicting multiple schedules.

2. **Invalid time input**
   - Times such as `25:00`, `-01:00`, or incorrect formats (`18-30`, text input).

3. **Duplicate schedules**
   - User creates the same ON/OFF schedule multiple times.

4. **Daylight Saving Time (DST) changes**
   - Schedule behavior when clocks move forward/backward.

5. **Time zone changes**
   - If the user travels or phone time zone changes.

### System Behavior Scenarios
6. **Manual override**
   - User manually turns light ON/OFF while a schedule exists.

7. **Offline device / network failure**
   - Smart hub or lights disconnected when schedule time occurs.

8. **App restart or device reboot**
   - Schedule persistence after reboot.

### Usability & Validation
9. **Editing existing schedules**
10. **Deleting schedules**
11. **Maximum number of schedules allowed**
12. **User feedback / confirmation messages**

---

# 3. Improved Professional QA Checklist

## Smart Home Lighting Schedule – QA Checklist

### 1. Basic Functionality
- User can create a schedule to **turn lights ON at a specified time**
- User can create a schedule to **turn lights OFF at a specified time**
- Scheduled action executes at the configured time
- Multiple schedules can be created for the same device

---

### 2. Schedule Management
- User can **edit an existing schedule**
- User can **delete an existing schedule**
- Schedule changes are **saved correctly after app restart**
- Schedule persists after **device reboot**

---

### 3. Input Validation
- System rejects **invalid time formats**
- System rejects **invalid times (e.g., 25:00, negative values)**
- System validates **duplicate schedules**
- System warns about **conflicting schedules**

---

### 4. Conflict Handling
- System handles **overlapping schedules**
- System resolves conflicts when **multiple rules trigger simultaneously**
- System behavior defined when **manual override occurs**

Example scenario:

| Scenario | Expected Result |
|--------|--------|
| Manual ON while schedule OFF at same time | System follows defined priority rule |
| Overlapping ON schedules | Lights remain ON without repeated commands |

---

### 5. Time Handling
- Schedule adjusts correctly for **Daylight Saving Time**
- Schedule adapts to **device time zone changes**
- Correct behavior when **system clock changes**

---

### 6. Connectivity & Reliability
- Scheduled event executes correctly when **device reconnects**
- System logs failures when **network unavailable**
- Retry mechanism if command fails

---

### 7. Usability (Mobile App)
- Schedule creation UI is **clear and intuitive**
- User receives **confirmation message after saving schedule**
- App displays **list of active schedules**
- User receives **error messages for invalid inputs**

---

### 8. Performance
- App supports **multiple schedules without delay**
- Schedule execution latency **within acceptable threshold**

---

# 4. Refined Requirement (Improved)

Original requirement:
> "The smart home app must allow users to set a daily schedule for turning lights on and off automatically."

### Improved Requirement

The smart home mobile application shall allow users to create, edit, and delete daily schedules for automatically turning smart lights ON or OFF at specified times.  
The system shall validate time inputs, prevent conflicting schedules, handle daylight saving and time zone changes, and maintain schedules after app restart or device reboot.  
The application shall provide clear user feedback for successful operations and errors.

---

# 5. Improved AI Prompt (for Better Test Generation)

Example improved prompt:
```text
"You are a Senior QA Engineer. Generate a complete test checklist for the following requirement:  
The smart home mobile app allows users to create daily schedules to turn lights ON or OFF automatically.  
Include:  
- Positive and negative test cases  
- Edge cases (invalid input, overlapping schedules, DST changes)  
- Manual override behavior  
- Mobile app usability validation  
- Reliability scenarios (offline devices, reconnecting)"
```
---

# 6. Comparison

| Aspect | Original Output | Improved Output |
|------|------|------|
| Coverage | Minimal | Comprehensive |
| Edge Cases | Missing | Included |
| Error Handling | None | Included |
| Usability | Not considered | Included |
| Reliability | Missing | Covered |

### Conclusion

The **refined requirement and prompt significantly improve AI output quality** by explicitly requesting edge cases, validation, and system behavior scenarios. This leads to a **more realistic QA checklist suitable for real-world testing**.