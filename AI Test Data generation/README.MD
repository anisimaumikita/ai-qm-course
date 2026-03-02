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