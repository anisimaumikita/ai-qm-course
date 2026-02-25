

# 🛠️ Exercise 3: Fixing Anti-Patterns

<p align="right">Date: <strong>2026-02-25</strong></p>

---

## 📝 Task

A junior QA on your team is trying to test an LLM's ability to review code for bugs. They use the following prompt:

> **Flawed Prompt:**
> 
> "Here is a Python function. Please find the obvious off-by-one indexing error in the 'for' loop and also look for the SQL injection vulnerability. Tell me what you find."

---

### ❓ Questions

- **Identify** which anti-pattern (discussed in the article) this prompt suffers from.
- **Explain** why this prompt makes it a bad or invalid test.
- **Rewrite** the prompt to be a neutral, effective test of the model's bug-finding capabilities, using a proper Role and Task.

---


## ✅ Answer

<details>
<summary><strong>Identify which anti-pattern(s) this prompt suffers from</strong></summary>

This prompt contains the following anti-patterns:

- <strong>Context Contamination</strong>
- <strong>Implicit Assumptions</strong>

</details>

<details>
<summary><strong>Explain why this prompt makes it a bad or invalid test</strong></summary>

### 1️⃣ Prompt Overloading

<em>Does it suffer from Prompt Overloading?</em>

🔸 <strong>Partially, but not primarily.</strong>

The prompt asks the model to:

- Find an off-by-one error
- Look for an SQL injection vulnerability

These are two distinct bug categories (logic + security), but they still fall under a single broader task: bug detection in code.

This is not severe prompt overloading like:

> “Analyze, write tests, generate data, and write automation code.”

So while the prompt includes multiple subtasks, they are closely related. The main issue is not overload.

### 2️⃣ Context Contamination ✅ (Primary Issue)

Yes — this is the main anti-pattern.

The prompt explicitly states:

> There is an off-by-one error
>
> There is an SQL injection vulnerability

This contaminates the test because:

- The model is no longer discovering bugs independently.
- It is being led to confirm specific issues.
- It may hallucinate those bugs if they don’t exist.
- It prevents objective evaluation of reasoning ability.
- Instead of testing diagnostic skill, it tests compliance with hints.

This directly matches the article’s definition of Context Contamination.

### 3️⃣ Implicit Assumptions

Yes — indirectly.

The prompt assumes:

- The off-by-one error is “obvious”
- An SQL injection vulnerability exists

These are unstated constraints about the code’s structure and flaws.

If those issues are not actually present, the model is being forced to operate under false assumptions.

However, compared to Context Contamination, this is a secondary issue.

</details>

### 🎯 Final Verdict

| Anti-Pattern            | Present? | Severity         |
|-------------------------|:--------:|:----------------|
| Prompt Overloading      | ⚠️ Mild  | Low             |
| Context Contamination   | ✅ Yes   | High (Primary)  |
| Implicit Assumptions    | ⚠️ Sec.  | Moderate        |

---

### 🚩 Why This Is a Bad Test

- Biases the model
- Prevents independent bug discovery
- Risks false positives
- Does not simulate real QA conditions

---




<details>
<summary><strong>Rewrite the prompt to be a neutral, effective test of the model's bug-finding capabilities, using a proper Role and Task</strong></summary>

#### 🆗 Neutral Prompt Example

**Role:** You are a code reviewer.

**Task:** Review the following Python function and identify any potential bugs or security vulnerabilities. Explain your reasoning for each issue you find.

```python
# Example function (replace with actual code to test)
def process_user_input(user_input):
	query = f"SELECT * FROM users WHERE name = '{user_input}'"
	# ... rest of the code ...
	return query
```

</details>

---



