# Exercise 1: QA Test Script Editing in Canvas

<p align="right">Date: <strong>2026-03-02</strong></p>

---
## 📝 Task

**Objective:** Practice using Canvas to edit and improve QA documentation or test scripts collaboratively.

**Instructions:**

1.Open a Canvas session

2.Copy an existing QA test script (can be a Selenium, Cypress, or unit test script) into the canvas

3.Highlight a block of code or a paragraph of the test documentation
4.Ask the assistant to:

  -Suggest improvements or refactor the code for readability

  -Identify potential bugs or edge cases that the test might miss

  -Add inline comments explaining each change or suggestion

5.Use the version control features to track at least two iterations of your edits

6.Export the final canvas as a Markdown or PDF

**Expected Result:**

  -The original test script plus at least two improved versions with inline comments

  -A clear changelog highlighting what was improved, refactored, or added

  -Reflection notes explaining why each change improves test quality or clarity
  

---

## ✅ Result
# QA Test Improvement Session -- Cypress Login Test

------------------------------------------------------------------------

## 🔹 Original Version (V1)

``` javascript
describe('Login Test', () => {
  it('should login successfully', () => {
    cy.visit('https://example.com/login')
    cy.get('#username').type('testuser')
    cy.get('#password').type('password123')
    cy.get('#loginBtn').click()
    cy.url().should('include', '/dashboard')
  })
})
```

------------------------------------------------------------------------

## 🔹 Improved Version (V2 -- Refactored for Readability & Best Practices)

``` javascript
// Group related login tests together
describe('Login Functionality', () => {

  // Use beforeEach to avoid repeating visit logic in future tests
  beforeEach(() => {
    cy.visit('https://example.com/login') // Centralized navigation step
  })

  it('should login successfully with valid credentials', () => {
    // Use environment variables instead of hardcoded credentials
    cy.get('#username').type(Cypress.env('username'))
    cy.get('#password').type(Cypress.env('password'), { log: false }) // Hide password from logs
    
    cy.get('#loginBtn').click()

    // Assert both URL and UI element for stronger validation
    cy.url().should('include', '/dashboard')
    cy.contains('Welcome').should('be.visible') // Verifies page content loaded
  })
})
```

### ✅ Improvements in V2

-   Added `beforeEach()` for scalability
-   Removed hardcoded credentials (security best practice)
-   Masked password logging
-   Added UI assertion beyond URL validation
-   Improved test naming clarity

------------------------------------------------------------------------

## 🔹 Improved Version (V3 -- Edge Cases & Robustness Added)

``` javascript
// Expanded test coverage including edge cases

describe('Login Functionality', () => {

  beforeEach(() => {
    cy.visit('https://example.com/login')
  })

  it('should login successfully with valid credentials', () => {
    cy.get('#username').type(Cypress.env('username'))
    cy.get('#password').type(Cypress.env('password'), { log: false })
    cy.get('#loginBtn').click()

    // Ensure dashboard fully loads before assertion
    cy.url().should('include', '/dashboard')
    cy.get('h1').should('contain.text', 'Dashboard')
  })

  it('should show error message with invalid credentials', () => {
    cy.get('#username').type('invalidUser')
    cy.get('#password').type('wrongPassword', { log: false })
    cy.get('#loginBtn').click()

    // Validate error handling behavior
    cy.contains('Invalid username or password').should('be.visible')
    cy.url().should('include', '/login') // Ensure user remains on login page
  })

  it('should disable login button when fields are empty', () => {
    // Validate client-side validation
    cy.get('#loginBtn').should('be.disabled')
  })

  it('should handle slow network response gracefully', () => {
    // Simulate delayed API response
    cy.intercept('POST', '/api/login', (req) => {
      req.on('response', (res) => {
        res.setDelay(2000) // Artificial delay
      })
    })

    cy.get('#username').type(Cypress.env('username'))
    cy.get('#password').type(Cypress.env('password'), { log: false })
    cy.get('#loginBtn').click()

    // Validate loading indicator appears
    cy.get('.loading-spinner').should('be.visible')
  })
})
```

------------------------------------------------------------------------

# 📌 Changelog

## Iteration 1 (V1 → V2)

-   Refactored structure using `beforeEach()`
-   Removed hardcoded credentials
-   Improved assertion strength
-   Improved test naming clarity
-   Added inline explanatory comments

## Iteration 2 (V2 → V3)

-   Added negative login test
-   Added form validation test
-   Added network delay simulation test
-   Increased reliability of dashboard validation
-   Improved coverage of edge cases

------------------------------------------------------------------------

# 🧠 Reflection Notes

### Why These Changes Improve Test Quality

**1. Scalability**\
Using `beforeEach()` makes the suite easier to expand without
duplicating navigation steps.

**2. Security & Maintainability**\
Using environment variables prevents credential exposure and improves
CI/CD compatibility.

**3. Stronger Assertions**\
Checking both URL and UI elements ensures the page is actually rendered
correctly.

**4. Edge Case Coverage**\
Testing invalid credentials and empty inputs ensures proper validation
behavior.

**5. Resilience to Real-World Conditions**\
Simulating slow network responses helps validate loading states and UX
behavior.

**6. Readability & Documentation**\
Inline comments make the test understandable for new QA engineers and
developers.
