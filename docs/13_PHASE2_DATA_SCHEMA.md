# 13 — Phase 2 Data Schema v2

## Purpose

Define canonical data outputs for Phase 2A source verification and candidate metabolic-deviation calculation.

---

## `source_verification_matrix.csv`

Required header:

```csv
species_name,source_name,source_type,exact_species_match,contains_metabolic_rate,contains_body_mass,contains_temperature,contains_physiological_state,usable_for_delta_e,reason_not_usable,citation_or_url,notes
```

Allowed `source_type` values:

```text
primary_literature
database
phylogeny_metadata
review
manual_note
```

---

## `allometry_config.json`

Top-level structure:

```json
{
  "version": "v2",
  "notes": "All equations must be verified before computation.",
  "equations": []
}
```

Equation object schema:

```json
{
  "equation_id": "string",
  "clade": "string",
  "equation_type": "PGLS | OLS | species_specific | clade_specific | other",
  "a": null,
  "b": null,
  "formula_text": "string",
  "mass_units": "g | kg",
  "metabolic_rate_units": "ml_O2_h | ml_O2_min | W | mg_O2_h | other",
  "log_space": false,
  "temperature_reference_c": null,
  "physiological_state": "BMR | SMR | RMR",
  "allowed_taxa": [],
  "citation": "string",
  "verified": false,
  "notes": "string"
}
```

Rules:

- `verified` must be true to compute residuals.
- Units must match or be converted before computation.
- Do not use entries with null `a` or `b` for computation.

---

## `real_delta_e_candidates.csv`

Required header:

```csv
taxon_id,species_name,clade,source_name,source_citation,exact_species_match,proxy_species_used,proxy_species_name,mass_measured_g,mass_source,metabolic_rate_measured,metabolic_rate_units,metabolic_rate_standardized_value,metabolic_rate_standardized_units,physiological_state,temp_measurement_c,temp_normalized_c,temperature_handling_method,allometry_equation_id,predicted_metabolic_rate,predicted_metabolic_rate_units,signed_log_residual,delta_E_magnitude,compute_eligible,quality_flags,notes
```

---

## Allowed compute states

Default compute-eligible states:

```text
BMR
SMR
RMR
```

Default non-compute states:

```text
FMR
MMR
AMR
DMR
active_swimming
postprandial
SDA
field
routine_unclear
unknown
```

---

## Required quality flags

Use semicolon-separated flags in CSV.

```text
FLAG_PROXY_SPECIES
FLAG_NON_STANDARD_STATE
FLAG_TEMP_MISSING
FLAG_TEMP_NORMALIZED
FLAG_TEMP_UNCORRECTED_REVIEW_REQUIRED
FLAG_TNZ_CONFIRMED
FLAG_TNZ_UNCLEAR
FLAG_TORPOR_RISK
FLAG_ARTIFICIAL_SELECTION_RISK
FLAG_MASS_NOT_PAIRED_TO_RATE
FLAG_UNVERIFIED_ALLOMETRY
FLAG_MISSING_CITATION
FLAG_COMPUTE_ELIGIBLE
FLAG_NOT_COMPUTE_ELIGIBLE
```

---

## `missing_data_log.md`

Required section template:

```md
## <Species name>

- Field missing:
- Status:
- Attempted sources:
- Available alternative data:
- Reason for exclusion:
- Fallback used:
- Human review needed:
- Impact on Phase 3 readiness:
```

---

## Core calculation

For compute-eligible rows only:

```python
signed_log_residual = log10(metabolic_rate_standardized_value / predicted_metabolic_rate)
delta_E_magnitude = abs(signed_log_residual)
```

Rows that are not compute-eligible must keep both fields null.
