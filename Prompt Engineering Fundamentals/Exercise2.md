# Exercise 2 — Validating Logic with Chain of Thought (CoT)

Date: 2026-02-25

Task: Validating Logic with Chain of Thought (CoT)
You are testing a complex piece of business logic for an e-commerce checkout. The rules are:

A 10% discount is applied if the cart total is over $100.
An additional 5% discount is applied if the user is a "Premium" member (this stacks with the first discount).
A $10 flat shipping fee is added to all orders under $50.
Shipping is free for orders $50 or more.

**Prompt**

```text
You are Senior QA Engineer and you need to test a complex piece of business logic for an e-commerce checkout. The rules of logic: -A 10% discount is applied if the cart total is over $100. -An additional 5% discount is applied if the user is a "Premium" member (this stacks with the first discount). -A $10 flat shipping fee is added to all orders under $50. -Shipping is free for orders $50 or more. Calculate the final price for three specific scenarios: Scenario A: A "Standard" user with a cart total of $120. Scenario B: A "Premium" user with a cart total of $120. Scenario C: A "Premium" user with a cart total of $40. Show You result in Table format and show your calculations step-by-step
```

Answer:

---

## Step-by-Step Calculations

### Scenario A: "Standard" user, cart total $120
- **Cart total:** $120
- **10% discount (over $100):** $120 × 10% = $12
- **Discounted total:** $120 - $12 = $108
- **No premium discount**
- **Shipping (over $50):** Free
- **Final price:** $108 + $0 = **$108**

### Scenario B: "Premium" user, cart total $120
- **Cart total:** $120
- **10% discount (over $100):** $120 × 10% = $12
- **Discounted total:** $120 - $12 = $108
- **Additional 5% premium discount:** $108 × 5% = $5.40
- **Discounted total:** $108 - $5.40 = $102.60
- **Shipping (over $50):** Free
- **Final price:** $102.60 + $0 = **$102.60**

### Scenario C: "Premium" user, cart total $40
- **Cart total:** $40
- **No 10% discount (under $100)**
- **No premium discount (no stacking)**
- **Shipping (under $50):** $10
- **Final price:** $40 + $10 = **$50**

---

## Summary Table

| Scenario   | User Type  | Cart Total | 10% Discount | 5% Premium Discount | Shipping | Final Price |
|:----------:|:----------:|:----------:|:------------:|:-------------------:|:--------:|:-----------:|
| A          | Standard   | $120       | $12          | $0                  | $0       | $108        |
| B          | Premium    | $120       | $12          | $5.40               | $0       | $102.60     |
| C          | Premium    | $40        | $0           | $0                  | $10      | $50         |

---
