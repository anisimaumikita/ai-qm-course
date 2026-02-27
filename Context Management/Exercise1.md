# 🧪 Exercise 1: Context Management

<p align="right">Date: <strong>2026-02-27</strong></p>

---

## 📝 Task

This exercise practices the "Iterative Prompting" and "Structuring Prompts" sections. Your goal is to fix a "flaky" prompt that gives chatty, unparseable output.

**1.Run the "Flaky" Prompt (V1):**
> Hey, can you give me 3 test users for my new website? I need a username, email, and a password for each.

**2.Analyze the Output:**
Observe the response. It will likely be conversational and unstructured (e.g., "Sure, here are three test users you can use!..."), which is a "flaky" test because it's not machine-readable and would fail a parsing script.

**3.Create a "Robust" Prompt (V2):**
Your task is to iteratively refine the prompt until it only outputs a valid JSON array.

- **Constraint 1:** The output must be only the JSON object, with no conversational text before or after it.
- **Constraint 2:** The JSON must be an array [] of three user objects.
- **Constraint 3:** Each object must have the keys: username, email, and password.

*Hint: Use strong, direct instructions as described in the article (e.g., "You are a test data generator," "Respond only with...", "Do not include...").*

---

### 💡 Good Prompt

```
You are a test data generator.
Generate exactly three test users.
Respond only with a valid JSON array.
Do not include any explanation, comments, markdown formatting, or conversational text before or after the JSON.
The JSON must:
- Be an array []
- Contain exactly three objects
- Each object must include exactly these keys: username, email, password
Use realistic but fake data
Output only the JSON array.
```

---

### 🧾 Result

```json
[
  {
    "username": "julia.morris",
    "email": "julia.morris@example.com",
    "password": "BlueSky!2026"
  },
  {
    "username": "david.chen",
    "email": "david.chen@example.com",
    "password": "RiverRun#42"
  },
  {
    "username": "sophia.garcia",
    "email": "sophia.garcia@example.com",
    "password": "Sunset*Lake9"
  }
]
```

---