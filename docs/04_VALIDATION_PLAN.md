# 04 — Validation Plan v2

## Purpose

Define gates that must pass before real-ΔE Phase 2 outputs can be used in Phase 3, and before any claims are updated.

The validation target is not “recover the sigmoid.” The validation target is **honest, auditable data grounding**.

---

## Execution metadata

| Field | Recommendation |
|---|---|
| Estimated time | 30–90 minutes after Phase 2 outputs exist |
| Model | Normal reasoning/coding model; Sonnet high only for ambiguous source disputes |
| Effort | Medium-high |
| Token risk | Low |
| Tools required | Python, pandas, JSON validation, manual source review |
| Cost caveat | No Claude Max/Fable needed for routine validation. |

---

## Validation Gate A — File existence

Required files:

```text
data/source_verification_matrix.csv
data/allometry_config.json
data/real_delta_e_candidates.csv
data/missing_data_log.md
```

If any are missing, Phase 3 cannot run.

---

## Validation Gate B — Schema validation

`real_delta_e_candidates.csv` must contain all fields defined in `13_PHASE2_DATA_SCHEMA.md`.

Required computed fields:

```text
metabolic_rate_standardized_value
metabolic_rate_standardized_units
predicted_metabolic_rate
signed_log_residual
delta_E_magnitude
quality_flags
compute_eligible
```

Rules:

- `signed_log_residual` must preserve positive/negative direction.
- `delta_E_magnitude` must equal `abs(signed_log_residual)`.
- Ineligible rows must have null residuals.
- Null values must stay null; do not fill them with estimated averages.

---

## Validation Gate C — Source validity

For each computed row:

- exact species match must be true,
- source citation must be present,
- measurement state must be BMR, SMR, or RMR,
- mass must be paired with the same metabolic-rate measurement or explicitly justified,
- measurement units must be convertible,
- allometry equation must be verified.

Fail the row if:

- it uses sister species data,
- it uses FMR/MMR/active/postprandial/diving-only data,
- it lacks measurement temperature when required,
- it lacks source citation,
- it uses unverified allometry constants.

---

## Validation Gate D — Allometry config

Every equation used for computation must include:

```text
equation_id
clade
equation_type
a
b
mass_units
metabolic_rate_units
log_space
temperature_reference_c or null
physiological_state
citation
verified=true
```

Reject if:

- constants appear directly in Python rather than config,
- units are missing,
- equation target clade is too broad for the row,
- equation produces units incompatible with measured data.

---

## Validation Gate E — Temperature and physiological condition

For endotherms:

- flag and review non-thermoneutral measurements,
- flag torpor/cold-stress risks,
- reject agricultural/artificially selected Gallus rows unless specifically intended as a separate artifact.

For ectotherms:

- measurement temperature must be present,
- correction/normalization must be cited if applied,
- if no correction is defensible, leave row non-computable.

Accepted flags:

```text
FLAG_TEMP_NORMALIZED
FLAG_TEMP_UNCORRECTED_REVIEW_REQUIRED
FLAG_TNZ_CONFIRMED
FLAG_TNZ_UNCLEAR
FLAG_TORPOR_RISK
FLAG_ARTIFICIAL_SELECTION_RISK
```

---

## Validation Gate F — Phase 3 readiness

Real-input Phase 3 may proceed only if:

1. At least 3 of 5 target species are compute-eligible.
2. No compute-eligible rows use proxy species.
3. No compute-eligible rows use FMR/MMR/DMR/active/postprandial data.
4. All compute-eligible rows pass unit and allometry checks.
5. All exclusions are recorded in `missing_data_log.md`.
6. Human approval is recorded.

If fewer than 3 target species are eligible, do not run Phase 3 as a claim-generating analysis. A dry-run may be allowed only as a provenance/debug artifact.

---

## If Phase 3 runs

Sensitivity grid remains required:

```text
lock-in threshold: 0.45, 0.50, 0.55
ratio epsilon: 1e-6, 1e-9, 1e-12
seeds: 42, 17, 73 if runtime allows
```

Outputs:

```text
outputs/phase3_real_delta_e_results.json
outputs/phase3_real_delta_e_sensitivity.json
outputs/phase3_real_delta_e_simulation.png
```

Required verdicts:

```text
stable
unstable
insufficient_data
blocked_data_gap
null_result
```

---

## Claim validation

Before updating README or CONTROL_PACK:

- confirm whether result is real-input or placeholder,
- include N and missing species,
- include sensitivity result,
- include null/no-threshold result if applicable,
- do not report a ratio headline alone,
- do not call the metric “ancestral transition energy.”
