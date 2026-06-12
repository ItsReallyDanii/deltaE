# 11 — Gemini Audit Integration Notes

## Purpose

Record what this v2 package accepts, rejects, and modifies from the Gemini audit of the Perplexity/Sonnet research memo.

This file is not a source of new biological truth. It is a control-package integration note.

---

## Accepted from Gemini

### 1. Rename the metric

Accepted.

The metric should not be described as true historical transition energy. It should be described as:

```text
Current Clade-Relative Metabolic Deviation
```

Reason: `log10(measured / predicted)` is cross-sectional. It compares a living species to a baseline equation. It does not reconstruct an ancestral state.

---

### 2. Preserve signed and absolute outputs

Accepted.

Required fields:

```text
signed_log_residual
delta_E_magnitude
```

Reason: sign preserves direction; magnitude supports bypass-cost comparison.

---

### 3. Do not hard-code allometry constants

Accepted.

All constants/equations must live in:

```text
data/allometry_config.json
```

with units, clade, equation type, citation, and `verified` flag.

---

### 4. Separate physiological states

Accepted.

Compute-eligible by default:

```text
BMR, SMR, RMR
```

Record but exclude by default:

```text
FMR, MMR, AMR, DMR, active_swimming, postprandial, SDA, field, routine_unclear
```

---

### 5. Add quality flags and missing-data logging

Accepted.

Every row must carry explicit quality/provenance flags.

---

### 6. Treat temperature as a validation gate

Partially accepted.

For ectotherms, temperature cannot be ignored. However, this package does not mandate a specific Q10/Arrhenius correction unless the correction is sourced and configured. If no defensible correction exists, the row should remain non-computable rather than be silently corrected.

---

## Rejected or softened from Gemini

### 1. “9.5/10 confidence”

Rejected.

This is too confident for a project with small N, data gaps, proxy risks, and unverified allometry equations.

Package confidence remains:

```text
medium for Phase 2A as an exploratory data-grounding step
low for any current biological threshold claim
```

---

### 2. “Absolute proof” style wording

Rejected.

No current result proves EME. Extreme outliers may be useful test cases, but they are not proof.

---

### 3. Revived “thermodynamic floor” language

Rejected.

The original thermodynamic-floor framing is superseded. The active frame is EME / bypass-cost / clade-relative metabolic deviation.

---

### 4. “Strictly complementary” novelty verdict

Softened.

Safe current wording:

```text
EME may be complementary to Dubiner/Cubo-style work, but novelty remains unverified until explicit differentiation and real-data outputs exist.
```

---

## New v2 implementation stance

The next implementation should produce source-verified data products first, not run Phase 3 immediately:

```text
data/source_verification_matrix.csv
data/allometry_config.json
data/real_delta_e_candidates.csv
data/missing_data_log.md
```

Only after these pass validation should Phase 3 receive real-input values.

---

## Human review required before

- using proxy/sister-species values,
- choosing temperature correction equations,
- marking allometry equations as verified,
- running Phase 3 with fewer than all 5 target species,
- updating README/CONTROL_PACK claims.
