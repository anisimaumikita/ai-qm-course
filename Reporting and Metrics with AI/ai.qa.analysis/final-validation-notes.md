# Validation Notes – AI QA Report

## What Was Verified

1. Total tests consistency:
   - Passed (92) + Failed (8) = 100 ✔
2. Module totals align with suite totals ✔
3. Defect counts correctly categorized ✔
4. Last 3 runs correctly selected: run-43, run-44, run-45 ✔
5. Fail/flake/retry values match raw JSON ✔
6. Infra alerts aligned with deployment timing ✔

## Clarifications

- Flaky tests (3) in regression suite are separate from run-level flake metrics.
- UAT regression spike correlated to jwt_refresh_v2 rollout + Redis CPU.
- PROD run shows major recovery, indicating non-permanent defect.

## Corrections Applied

- Ensured failure % calculations were accurate per module.
- Ensured regression defined as increase 70 → 80.
- Ensured forecast grounded in measurable trends (not speculative).

No data conflicts detected.