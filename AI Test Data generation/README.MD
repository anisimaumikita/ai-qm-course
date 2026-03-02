## Chapter 1 – Customer Order Dataset

Prompt used: "Generate 15 customer orders with fields: orderId, customerName, status (Pending, Shipped, Cancelled), and totalAmount. Return as JSON array."

Observations:
- Order totals are realistic but currency formatting is inconsistent (mixed decimal precision).
- No financial edge cases (0.00, very large values, negative values).
- Customer names are realistic but lack special characters and boundary-length cases.
- Status values are valid and reasonably distributed.
- No duplicate or nonsensical data detected.
- Dataset is suitable for basic UI/API happy-path testing.

## Chapter 2 – Extended Customer Order Dataset

Prompt used: "Update and generate 30 additional customers. 
Extend your schema by adding the following new fields to each record: 
- email — a realistic address that matches the customer name pattern. 
- phoneNumber — a phone number string using a consistent pattern such as +0-000-000-0001. 
- items — an array of objects (name, quantity, price) describing purchased products. 

Also follow the next rules: 
- Expanded statuses (for example: Returned, On Hold, Payment Error, Delayed).
- Boundary totals (0, very large, and a few negative values for negative testing). 
- A small set of invalid entries (missing fields or wrong data types). 
- The new contact and item fields populated with realistic values."

Observations:
- Expanded status set improves workflow and state-transition coverage.
- Boundary totals included (0.00, very large, negative values) for financial edge testing.
- Small set of intentionally invalid entries (wrong data type, missing field, malformed email) supports negative testing.
- Email and phone number patterns are consistent and realistic for validation checks.
- Items array structure enables cart-level and total-calculation validation.
- Dataset is suitable for API contract testing, validation rules, and automation scenarios.

## Chapter 3 – PII Masking & Validation Results

Prompt used: "Mask PII in the dataset: - Replace customerName, email, and phoneNumber with synthetic but realistic values. - Keep orderId, status, totalAmount, and items unchanged. - Preserve types and overall structure. - Output as JSON in the same schema."

Observations:
- PII fields (customerName, email, phoneNumber) successfully replaced with synthetic but realistic values.
- Data structure and non-PII business fields remained unchanged, preserving test integrity.
- Email and phone formats conform to consistent validation patterns.
- Validation detected schema mismatch: orderId remains a string instead of expected integer.
- Validation flagged incorrect totalAmount type (string) and missing items field in one record.
- Dataset now supports safe sharing while still enabling negative and contract testing scenarios.

## Chapter 4 – Postman Runner Execution Analysis

Test Run Summary:
- Total iterations: 30
- Total Pass: 120
- Total Fail: 120
- Overall status: Finished
- Response code: 200 OK for all requests
- Average response time: ~219ms (stable performance)

Observations:
- API endpoint (postman-echo) responded successfully for all requests (no transport failures).
- "Round-trip integrity" passed for all 30 iterations, confirming payload echo consistency.
- "Schema checklist compliance" passed for all iterations, indicating baseline schema alignment.
- "Structural keys present" failed for all 30 iterations, suggesting required fields were missing or improperly referenced during validation.
- "Type checks" failed consistently, likely due to:
  - orderId being string instead of integer.
  - totalAmount string type in one record.
  - Missing items field in one record.
- Failures are systematic (59 failures per failing test), indicating validation rule mismatch rather than random data corruption.

Conclusion:
- API transport layer is stable.
- Validation layer correctly detected schema/type inconsistencies.
- Dataset intentionally triggered negative test coverage as designed.
- Collection is suitable for contract and schema enforcement testing but requires alignment between expected schema (integer orderId) and actual payload structure.

## Chapter 5 – AI Data Quality Report

Validation source: Postman  
Total records tested: 30  
Total assertions: 240  
Passed assertions: 120  
Failed assertions: 120  

Assertion pass rate: 50 %

---

### Failure Summary

Records affected:
- Structural key validation: 30
- Type validation: 30

Field-level issues:
- orderId type mismatch (string vs expected integer): 30
- totalAmount string instead of number: 1
- Missing items field: 1
- Status / email / phone errors: 0

---

### Observations
- Failures are systematic due to schema mismatch (orderId type).
- Negative and boundary values behaved as expected.
- Masking preserved valid field structure.

---

### Next Steps
- Align orderId type with validator.
- Enforce strict numeric typing in prompts.
- Separate compliant and negative-test datasets.
- Add pre-validation before API submission.