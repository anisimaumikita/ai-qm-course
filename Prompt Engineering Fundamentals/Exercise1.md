# Exercise 1 — From Vague to Structured (Test Data Generation)

Date: 2026-02-25

Task: From Vague to Structured (Test Data Generation)

"Bad" Prompt: "Give me test data for the user profile endpoint."

"Good" Prompt:
You are a Senior QA specialist with expertise in testing data, API validation, boundary value analysis, positive and negative testing strategies. You need to generate test data for a new API endpoint that creates user profiles in JSON array format and cover the next requirements: username: string, minimum 8 characters, alphanumeric only. email: string, must be a valid email format. user_role: string, must be one of three values: "admin", "editor", or "viewer". age: integer, between 18 and 99 
Generate 5 unique negative test cases

Prompt result:

Negative Test Case 1 – Username below minimum length

Validation Targeted: username minimum 8 characters

[
  {
    "username": "user12",
    "email": "validuser1@example.com",
    "user_role": "admin",
    "age": 25
  }
]

Expected Result:
❌ 400 Bad Request – "username must be at least 8 characters"

Negative Test Case 2 – Username contains non-alphanumeric characters

Validation Targeted: username alphanumeric only

[
  {
    "username": "user_1234",
    "email": "validuser2@example.com",
    "user_role": "editor",
    "age": 30
  }
]

Expected Result:
❌ 400 Bad Request – "username must be alphanumeric only"

Negative Test Case 3 – Invalid email format

Validation Targeted: email format validation

[
  {
    "username": "validuser3",
    "email": "invalid-email-format",
    "user_role": "viewer",
    "age": 40
  }
]

Expected Result:
❌ 400 Bad Request – "email format is invalid"

Negative Test Case 4 – Invalid user_role value

Validation Targeted: user_role enum constraint

[
  {
    "username": "validuser4",
    "email": "validuser4@example.com",
    "user_role": "superadmin",
    "age": 28
  }
]

Expected Result:
❌ 400 Bad Request – "user_role must be admin, editor, or viewer"

Negative Test Case 5 – Age below allowed boundary

Validation Targeted: age boundary value (minimum 18)

[
  {
    "username": "validuser5",
    "email": "validuser5@example.com",
    "user_role": "viewer",
    "age": 17
  }
]

Expected Result:
❌ 400 Bad Request – "age must be between 18 and 99"


---
