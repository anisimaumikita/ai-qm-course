# Exercise 1 — From Vague to Structured (Test Data Generation)

Date: 2026-02-25

Task: From Vague to Structured (Test Data Generation)

**Bad prompt**

```text
Give me test data for the user profile endpoint.
```

**Good prompt**

```text
You are a Senior QA specialist with expertise in testing data, API validation, boundary value analysis, positive and negative testing strategies.
Generate 5 unique negative test cases in JSON array format for a user profile endpoint with these requirements:
- username: string, minimum 8 characters, alphanumeric only
- email: string, valid email format
- user_role: string, one of "admin", "editor", or "viewer"
- age: integer, between 18 and 99
```

**Prompt result — Negative test cases (JSON array)**

```json
[
  {
    "username": "user12",
    "email": "validuser1@example.com",
    "user_role": "admin",
    "age": 25,
    
  },
  {
    "username": "user_1234",
    "email": "validuser2@example.com",
    "user_role": "editor",
    "age": 30,
    
  },
  {
    "username": "validuser3",
    "email": "invalid-email-format",
    "user_role": "viewer",
    "age": 40,
    
  },
  {
    "username": "validuser4",
    "email": "validuser4@example.com",
    "user_role": "superadmin",
    "age": 28,
    
  },
  {
    "username": "validuser5",
    "email": "validuser5@example.com",
    "user_role": "viewer",
    "age": 17,
    
  }
]
```

**Expected errors (correspond to the test cases in the JSON array, in order)**

1. username must be at least 8 characters
2. username must contain only alphanumeric characters
3. email must be a valid email address
4. user_role must be one of ["admin", "editor", "viewer"]
5. age must be between 18 and 99

---
