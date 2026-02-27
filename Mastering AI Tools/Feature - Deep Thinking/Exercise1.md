# Exercise 3: Scenario - Token Budgeting and Truncation

<p align="right">Date: <strong>2026-02-27</strong></p>

---

## 📝 Task

Use both **Quick** and **Thinking** modes to respond to this prompt:
```text
Create a risk-based test plan for a file upload feature that supports multiple file types and size limits.
```
Compare results for:
-Test coverage and edge cases identified
-Clarity of test prioritization
-Justification of test scope

Explain when Thinking mode improves QA outcomes versus when Quick mode is enough.

---


## ✅ Result

# Quick vs Thinking Mode – Risk-Based Test Plan Comparison

## 1. Test Coverage & Edge Cases Identified

### ✅ Quick Mode (First Answer)

#### Strengths

- Covers major risk areas:
  - Security
  - Performance
  - Validation
  - Boundary testing

- Identifies critical edge cases:
  - Double extensions (`file.pdf.exe`)
  - MIME spoofing
  - Path traversal
  - Size boundaries (`max-1`, `max`, `max+1`)
  - 0-byte file
  - Concurrent uploads

- Includes:
  - Risk table
  - Prioritization levels
  - Security validation depth
  - Automation strategy
  - Entry/Exit criteria

#### Limitations

- Assumes generic system context (no architecture-specific risk tailoring)
- Does not differentiate between:
  - Cloud vs on-prem storage risks
  - API-only vs UI-based uploads
  - Compliance-driven constraints
- Does not explicitly map risks to business impact scenarios:
  - Revenue loss
  - Legal exposure

#### Overall Coverage Quality

Strong and production-ready for most enterprise systems.

---

### ⚠️ Research / Thinking Mode (Second Answer – Clarifying Questions)

#### Strengths

- Seeks contextual risk inputs before producing a plan.
- Aims to tailor risk analysis based on:
  - Authentication model
  - Compliance requirements
  - Platform scope
  - File types and size constraints

- Would potentially produce:
  - More context-aware risks
  - More accurate prioritization
  - Better regulatory alignment

#### Limitations

- Provides no immediate test plan.
- No edge cases listed.
- No coverage demonstrated yet.

#### Overall Coverage Quality

Incomplete as delivered. Potentially superior if followed by contextualized output.

---

## 2. Clarity of Test Prioritization

### ✅ Quick Mode

Very clear prioritization structure:

- **P1** – Security & Stability  
- **P2** – Performance & Reliability  
- **P3** – UX & Compatibility  

Includes:

- Risk matrix (Impact × Likelihood)
- Direct traceability mapping (Risk → Test Area)
- Clear “If time is limited, test in this order” section

**Clarity Level:** High  

A QA team could execute immediately.

---

### ⚠️ Research Mode

- No prioritization provided yet.

However, intent suggests prioritization would be tailored based on:

- Public vs authenticated users
- Regulatory risk
- File sensitivity

**Clarity Level:** Not yet demonstrated

---

## 3. Justification of Test Scope

### ✅ Quick Mode

Scope justification is implicit via:

- Risk table
- Security-first focus
- Performance impact scenarios
- Storage exhaustion risk

However:

- Justification is generic, not business-contextual.
- Does not explain why a specific organization might rank risks differently.

**Scope Justification Quality:** Good but generalized.

---

### ⚠️ Research Mode

Justification strategy would likely be stronger because it asks about:

- Compliance (GDPR, HIPAA)
- Public vs authenticated access
- Platform scope

This enables risk justification based on:

- Legal penalties
- Data sensitivity
- Business exposure

**Scope Justification Potential:** High (if followed through)

---

## 4. Side-by-Side Summary

| Dimension | Quick Mode | Research / Thinking Mode |
|------------|------------|--------------------------|
| Immediate usability | ✅ High | ❌ Low |
| Edge case depth | ✅ Strong | ❌ None yet |
| Security coverage | ✅ Strong | ❓ TBD |
| Context awareness | ⚠️ Generic | ✅ High (if continued) |
| Risk prioritization clarity | ✅ Clear P1/P2/P3 | ❌ Not provided |
| Enterprise readiness | ✅ Ready to use | ❌ Requires follow-up |
| Strategic customization | ⚠️ Limited | ✅ Potentially strong |

---

## 5. When Thinking Mode Improves QA Outcomes

Thinking Mode is better when:

### 1. Regulatory or Legal Exposure Exists

Examples:

- Healthcare system (HIPAA)
- Financial uploads (PCI)
- Government platforms

Misprioritizing risk could cause compliance failure.

---

### 2. Architecture Matters

- CDN-backed uploads
- Pre-signed URLs
- Chunked uploads
- Cloud object storage

Different architecture = different risk profile.

---

### 3. Business Context Changes Risk

- Public anonymous uploads → higher abuse risk
- Internal enterprise tool → performance risk may outweigh security
- Revenue depends on upload reliability → performance becomes P1

---

### 4. Large Distributed Systems

Complex ecosystems require:

- Threat modeling
- Failure mode analysis
- Storage scalability assessment

---

## 6. When Quick Mode Is Enough

Quick Mode is sufficient when:

### ✅ Standard Web Application

- Authenticated users only
- Common file types (PDF, image, DOCX)
- Defined size limits
- No heavy compliance burden

### ✅ Interview Scenarios

- QA manager interviews
- Test strategy assessments
- Certification exams

### ✅ Early-Phase Planning

- Sprint planning
- Initial test scoping
- Proof-of-concept features

### ✅ Time-Constrained Environments

- Rapid risk assessment
- Bug triage support

---

## 7. Key Insight

### Quick Mode = Tactical  
### Thinking Mode = Strategic  

**Quick Mode:**

- Executes known patterns
- Applies standard best practices
- Works well when the domain is common

**Thinking Mode:**

- Adjusts risk model to context
- Improves prioritization accuracy
- Reduces over-testing low-risk areas
- Prevents blind spots in specialized systems

---

## 8. Final Evaluation

For this specific prompt:

The **Quick Mode answer was stronger** because:

- The domain (file upload with type & size validation) is common.
- Risk patterns are well-known.
- A generic risk model was sufficient.

Thinking Mode would outperform Quick Mode only if:

- The system had unique constraints.
- There were regulatory or architectural complexities.
- The upload feature had business-critical implications.