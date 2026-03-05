# Train Ticket Booking – QA Test Design & Review

## Acceptance Criteria
Passengers must be able to book train tickets online. After successful payment, the system must generate an electronic ticket with a unique booking reference. If the selected train is already full, the system must prevent booking and display an appropriate error message.

---

# 1. QA Checklist

## Functional Validation
- [ ] User can search for available trains.
- [ ] User can select a train with available seats.
- [ ] User can proceed to booking after selecting a train.
- [ ] System allows entering passenger details.
- [ ] System supports payment processing.
- [ ] System confirms booking after successful payment.
- [ ] System generates an electronic ticket after booking.
- [ ] Electronic ticket contains a unique booking reference.
- [ ] Booking reference format is valid and not duplicated.
- [ ] Ticket includes correct passenger and train information.

## Error Handling
- [ ] System prevents booking when the train is full.
- [ ] Appropriate error message is displayed when seats are unavailable.
- [ ] System prevents duplicate payment attempts.
- [ ] System handles payment failure correctly.

## Edge Cases
- [ ] Two users attempt to book the last seat simultaneously.
- [ ] Payment succeeds but ticket generation fails.
- [ ] Network interruption during payment.
- [ ] Booking session expires before payment completion.

## Security & Compliance
- [ ] Payment flow uses secure transactions.
- [ ] Booking reference cannot be guessed or manipulated.
- [ ] Personal data is protected and not exposed in logs.

## Usability
- [ ] Error messages are clear and user-friendly.
- [ ] Ticket is accessible via email or download.
- [ ] Booking confirmation page displays ticket details.

---

# 2. Test Cases

| ID | Test Case | Preconditions | Steps | Expected Result |
|---|---|---|---|---|
| TC-01 | Successful train ticket booking | Train has available seats | Search train → Select train → Enter passenger details → Complete payment | Booking confirmed and electronic ticket generated |
| TC-02 | Unique booking reference generation | Booking completed | Perform two separate bookings | Each ticket has a unique booking reference |
| TC-03 | Prevent booking when train is full | Train capacity reached | Attempt booking | Booking blocked |
| TC-04 | Error message for full train | Train full | Attempt booking | System displays "Train is fully booked" message |
| TC-05 | Payment failure scenario | Payment gateway available | Enter invalid payment details | Payment rejected and booking not created |
| TC-06 | Ticket data validation | Successful booking | Open electronic ticket | Ticket contains passenger name, train details, booking reference |
| TC-07 | Concurrent booking for last seat | One seat remaining | Two users attempt booking simultaneously | Only one booking succeeds |
| TC-08 | Session timeout during booking | Session timeout configured | Wait until session expires then continue booking | Session expired message shown |
| TC-09 | Network interruption during payment | Simulated network issue | Disconnect network during payment | Transaction handled safely |
| TC-10 | Duplicate payment prevention | Payment submitted once | Resubmit payment request | System prevents duplicate charge |

---

# 3. Test Plan

## Objective
Verify that passengers can book train tickets online, the system generates an electronic ticket with a unique booking reference after successful payment, and booking is prevented when the train is full.

---

## Scope

### In Scope
- Train search and selection
- Passenger data entry
- Payment processing
- Electronic ticket generation
- Booking reference generation
- Full train booking prevention
- Error message display

### Out of Scope
- Internal logic of third-party payment providers
- External notification services

---

## Test Strategy

### Testing Types
- Functional testing
- Integration testing (payment gateway)
- Negative testing
- Concurrency testing
- Security testing
- Usability testing

### Test Levels
- UI testing
- API testing
- End-to-end testing

---

## Test Environment
- Staging environment
- Web browsers (Chrome, Firefox, Edge)
- Payment gateway sandbox
- Test database with configurable train capacity

---

## Test Data
- Trains with available seats
- Fully booked trains
- Passenger profiles
- Valid and invalid payment methods

---

## Entry Criteria
- Feature deployed in test environment
- Payment sandbox available
- Test data prepared

---

## Exit Criteria
- All critical tests executed
- No critical defects open
- Booking flow verified end-to-end

---

## Risks
- Overbooking due to race conditions
- Payment gateway instability
- Ticket generation service failure

---

## Deliverables
- Test cases
- Bug reports
- Test execution report
- QA sign-off

---

# 4. QA Output Review

## 1. Did it include both successful bookings and the case where a train is full?

Yes.

The generated test design includes both positive and negative scenarios.

### Successful booking scenarios
- TC-01 – Successful booking after payment.
- TC-02 – Unique booking reference verification.
- TC-06 – Ticket content validation.

### Full train scenario
- TC-03 – Prevent booking when train is full.
- TC-04 – Error message validation for full train.

### Concurrency scenario
- TC-07 – Two users attempting to book the last seat simultaneously.

This ensures validation of seat availability logic and prevents overbooking.

---

## 2. Did it cover variations like invalid passenger details, duplicate bookings, or payment errors?

Partially.

### Covered variations
- Payment failure scenario (TC-05)
- Duplicate payment prevention (TC-10)
- Network interruption during payment (TC-09)
- Session timeout (TC-08)
- Concurrency issues (TC-07)

### Missing scenarios
- Invalid passenger details
- Passenger data validation rules
- Duplicate booking attempts by the same user
- Validation for required passenger fields

These scenarios should be added to improve test coverage.

---

## 3. Does it include scope, objectives, and risks?

Yes.

### Scope
Defined clearly in the test plan.

In scope:
- Online booking
- Passenger details input
- Payment process
- Ticket generation
- Seat availability validation

Out of scope:
- Payment gateway internal logic
- Third-party notification services

---

### Objectives
The testing objective includes:

- Validating online ticket booking
- Ensuring ticket generation after payment
- Verifying unique booking references
- Ensuring correct seat availability handling

---

### Risks
The plan identifies several realistic risks:

- Overbooking due to race conditions
- Payment gateway instability
- Ticket generation service failure

---

# Final Evaluation

Overall the generated QA design is structured and close to production-level documentation.

## Strengths
- Covers positive and negative scenarios
- Includes concurrency testing
- Contains a structured test plan
- Identifies realistic system risks

## Areas for Improvement
- Add passenger data validation tests
- Add duplicate booking prevention scenarios
- Include API-level validation tests
- Add performance testing for peak booking periods