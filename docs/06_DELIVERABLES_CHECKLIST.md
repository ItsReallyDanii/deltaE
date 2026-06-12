# 06 — Deliverables Checklist v2

## Purpose

Define what must exist before implementation can advance from Phase 2 data grounding to Phase 3 simulation.

---

## Phase 2A required deliverables

| Deliverable | Required? | Pass condition |
|---|---:|---|
| `data/source_verification_matrix.csv` | Yes | All target/reference species assessed against source hierarchy. |
| `data/allometry_config.json` | Yes | No hard-coded constants; every usable equation has units/citation/verified flag. |
| `data/real_delta_e_candidates.csv` | Yes | Schema-valid; ineligible rows have null residuals. |
| `data/missing_data_log.md` | Yes | Every missing/proxy/excluded value documented. |
| validation script or notebook | Strongly recommended | Checks schema, units, flags, and compute eligibility. |

---

## Phase 2A pass/fail gate

Phase 3 can be considered only if:

- at least 3 of 5 target species are compute-eligible,
- no compute-eligible row uses proxy species,
- no compute-eligible row uses FMR/MMR/DMR/active/postprandial data,
- allometry equations are verified in config,
- ectotherm temperature handling is explicit,
- human approval is recorded.

If the gate fails, deliver a blocked-data-gap report instead.

---

## Phase 3 deliverables, only if gate passes

| Deliverable | Required? | Pass condition |
|---|---:|---|
| `outputs/phase3_real_delta_e_results.json` | Yes | Uses real candidate CSV, not placeholder array. |
| `outputs/phase3_real_delta_e_sensitivity.json` | Yes | Threshold/epsilon/seed grid present. |
| `outputs/phase3_real_delta_e_simulation.png` | Optional | Labels real-input vs placeholder clearly. |
| README/CONTROL_PACK patch | Yes after validation | Uses cautious claim tags. |

---

## Completion states

Acceptable final states:

```text
PHASE2A_PASS_READY_FOR_HUMAN_REVIEW
BLOCKED_DATA_GAP
INSUFFICIENT_COMPUTABLE_N
PHASE3_REAL_INPUT_NULL_RESULT
PHASE3_REAL_INPUT_UNSTABLE
PHASE3_REAL_INPUT_PRELIMINARY_PATTERN
```

Unacceptable final states:

```text
CONFIRMED_THRESHOLD_FROM_PLACEHOLDER
PROXY_VALUES_USED_SILENTLY
HARD_CODED_ALLOMETRY_CONSTANTS
PUBLISHED_CLAIM_WITHOUT_SENSITIVITY
```
