---

## Step-by-Step Calculations

| Scenario | User Type | Cart Total | Discount Applied | Subtotal After Discount | Shipping Fee | Final Price |
|----------|-----------|------------|------------------|------------------------|--------------|-------------|
| A        | Standard  | $120       | 10% ($12)        | $108                   | Free         | $108        |
| B        | Premium   | $120       | 10% ($12) + 5% ($5.40) | $102.60          | Free         | $102.60     |
| C        | Premium   | $40        | None             | $40                    | $10          | $50         |

### Scenario A: Standard user, cart total $120
1. Cart total > $100 → 10% discount: $120 × 10% = $12
2. Subtotal after discount: $120 - $12 = $108
3. User is not Premium → no extra discount
4. Subtotal $108 ≥ $50 → Free shipping
5. **Final price: $108**

### Scenario B: Premium user, cart total $120
1. Cart total > $100 → 10% discount: $120 × 10% = $12
2. Subtotal after first discount: $120 - $12 = $108
3. Premium user → additional 5% discount: $108 × 5% = $5.40
4. Subtotal after both discounts: $108 - $5.40 = $102.60
5. Subtotal $102.60 ≥ $50 → Free shipping
6. **Final price: $102.60**

### Scenario C: Premium user, cart total $40
1. Cart total ≤ $100 → no discount
2. Subtotal: $40
3. Premium user → no discount (cart total not > $100)
4. Subtotal $40 < $50 → $10 shipping fee
5. **Final price: $40 + $10 = $50**

# Exercise 2

**Date:** 2026-02-25

## Task: Validating Logic with Chain of Thought (CoT)

You are testing a complex piece of business logic for an e-commerce checkout. The rules are:

- **10% discount** is applied if the cart total is over $100.
- **Additional 5% discount** is applied if the user is a "Premium" member (this stacks with the first discount).
- **$10 flat shipping fee** is added to all orders under $50.
- **Shipping is free** for orders $50 or more.

---

## Answer

✅ Business Rules Summary

10% discount → Applied if cart total > $100

Additional 5% discount → Applied if user is Premium (stacks with first discount)

$10 shipping fee → Applied if final cart total < $50

Free shipping → If order total ≥ $50

Assumption: Discounts are applied to cart subtotal first, then shipping is added based on the final discounted subtotal.

🔎 Scenario Calculations
🅰 Scenario A

User Type: Standard
Cart Total: $120

Step 1: Apply 10% discount (since $120 > $100)
120
×
10
%
=
12
120×10%=12
120
−
12
=
108
120−12=108
Step 2: Premium discount?

No (Standard user)

Subtotal after discount = $108

Step 3: Shipping

Since $108 ≥ $50 → Free Shipping

✅ Final Total = $108
🅱 Scenario B

User Type: Premium
Cart Total: $120

Step 1: Apply 10% discount
120
×
10
%
=
12
120×10%=12
120
−
12
=
108
120−12=108
Step 2: Apply additional 5% Premium discount (stacked)
108
×
5
%
=
5.40
108×5%=5.40
108
−
5.40
=
102.60
108−5.40=102.60

Subtotal after discounts = $102.60

Step 3: Shipping

Since $102.60 ≥ $50 → Free Shipping

✅ Final Total = $102.60
🅲 Scenario C

User Type: Premium
Cart Total: $40

Step 1: 10% discount?

No (cart not over $100)

Step 2: Apply 5% Premium discount
40
×
5
%
=
2
40×5%=2
40
−
2
=
38
40−2=38

Subtotal after discount = $38

Step 3: Shipping

Since $38 < $50 → Add $10 shipping

38
+
10
=
48
38+10=48
✅ Final Total = $48
📊 Final Results Table
Scenario	User Type	Original Cart	10% Discount	5% Premium Discount	Subtotal After Discounts	Shipping	Final Total
A	Standard	$120	-$12.00	$0.00	$108.00	$0	$108.00
B	Premium	$120	-$12.00	-$5.40	$102.60	$0	$102.60
C	Premium	$40	$0.00	-$2.00	$38.00	$10	$48.00
🧪 QA Validation Notes

✔ Discount stacking works correctly (10% applied before 5%)

✔ Shipping rule evaluated after discounts

✔ Boundary conditions validated:

-Over $100 discount trigger

-Under $50 shipping trigger

✔ No negative totals produced


