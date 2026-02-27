# Exercise 3: Scenario - Token Budgeting and Truncation

<p align="right">Date: <strong>2026-02-27</strong></p>

---

## 📝 Task

This exercise is a "what-if" scenario based on the "Token Budgeting" section.

**The Scenario:**

You are running a long, multi-step test that validates an e-commerce workflow. The model's context window is 4,096 tokens.

- **System Prompt:** Your global system prompt ("You are a test assistant...") and few-shot examples take up 1,000 tokens. (This is always present).
- **Step 1 (Login):** Your prompt and the model's response take up 500 tokens.
- **Step 2 (Add Item):** Your prompt and response take up 500 tokens.
- **Step 3 (Add 2nd Item):** Your prompt and response take up 500 tokens.
- **Step 4 (Go to Cart):** Your prompt and response take up 500 tokens.

---

## The Problem

You are now at **Step 5 (Validate Cart)**. Before you send your new prompt for Step 5, your total context is:

```text
1000 (System) + 500 (Step 1) + 500 (Step 2) + 500 (Step 3) + 500 (Step 4) = 3000 tokens
```

Your new prompt for Step 5 (which includes pasting the entire contents of the cart page) is very large: 2,000 tokens.

If you send this, your total context will be:

```text
3000 (History) + 2000 (New Prompt) = 5000 tokens
```

This is over the 4,096 limit.

---

## Your Task

Based on the article, describe the consequence of this action and choose a strategy to fix it.

- **What is the "dangerous consequence"** of sending this 5,000-token prompt, according to the article?  
  (Hint: It involves the "oldest" messages).

- **Which strategy** from the "Token Budgeting" section would you implement, and why?

  - **Windowing:** (e.g., only keep the last 2 messages).
  - **Summarization:** (e.g., summarize the outcome of steps 1–4).
  - **Pruning:** (e.g., keep the System Prompt but manually delete Steps 1, 2, and 3 from the history).

---

## ✅ Result

**Dangerous consequence:**  
The model will automatically truncate the oldest messages — likely removing the System Prompt — causing it to forget instructions and behave unpredictably.

**Best strategy:**  
Summarization, because it preserves essential context while staying within the 4,096-token limit.