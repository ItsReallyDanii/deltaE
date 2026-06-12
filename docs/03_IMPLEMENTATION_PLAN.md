# 03 — Implementation Plan v2: Phase 2A Data Verification Before Any Phase 3 Rerun

## Purpose

Build a reproducible, auditable Phase 2 data-grounding path for EME.

This v2 plan incorporates the Perplexity/Sonnet research memo and the Gemini audit. The most important correction is that the current metric is **not** a true ancestral transition-energy measurement. It is a **Current Clade-Relative Metabolic Deviation** proxy.

Do **not** run Phase 3 until the Phase 2A data-quality gate passes.

---

## Execution metadata

| Field | Recommendation |
|---|---|
| Estimated human setup time | 15–30 minutes |
| Estimated coding time | 2–5 hours for Phase 2A source verification + schema |
| Model | Normal coding agent / Hermes / Claude Code Sonnet normal or high |
| Effort | Medium for code; high only for source/equation ambiguity |
| Thinking/reasoning | Useful for data provenance and stop/go decisions |
| Token risk | Low-to-medium; repo is small |
| Tools required | Python, pandas, numpy, requests, CSV/JSON, local files |
| API/data costs | Likely free public sources; some manual literature/dataset download may be needed |
| Cost caveat | Do not use Claude Max/Fable for this initial implementation. |

---

## Updated metric definition

Preferred exploratory proxy:

```text
signed_log_residual = log10(metabolic_rate_measured_standardized / metabolic_rate_predicted_from_clade_baseline)
delta_E_magnitude = abs(signed_log_residual)
```

Required naming:

```text
Current Clade-Relative Metabolic Deviation
```

Forbidden naming unless ancestral-state reconstruction is later implemented:

```text
ancestral-vs-current ΔE
historical transition energy
true evolutionary transition magnitude
```

---

## Phase 2A goal: source/data verification only

Create these files first:

```text
data/source_verification_matrix.csv
data/allometry_config.json
data/real_delta_e_candidates.csv
data/missing_data_log.md
```

Do not modify Phase 3 simulation behavior until these exist and pass validation.

---

## Step 0 — Preserve scope

Target species remain:

```text
Crocodylus porosus
Alligator mississippiensis
Heterocephalus glaber
Dermochelys coriacea
Thunnus thynnus
```

Reference/outgroup taxa remain:

```text
Gallus gallus
Mus musculus
```

Do not add species unless writing a separate recommendation file. Do not silently substitute sister species.

---

## Step 1 — Source hierarchy

### Do not use as primary metabolic-rate sources

| Source | Role |
|---|---|
| AmphiBIO | Not suitable for the current target set; may be recorded as assessed/not used. |
| EltonTraits | Not a metabolic-rate source; may help only with limited bird/mammal ecological/body-mass context. |
| TimeTree | Divergence-time metadata only; not metabolic-rate data. |

### Preferred source classes

| Source class | Role |
|---|---|
| Primary literature | Highest priority for exact species metabolic measurements. |
| AnimalTraits | Candidate source for body mass / BMR / RMR where metadata are sufficient. |
| MetaR | Candidate source for ectotherm SMR/RMR/MMR metadata, including measurement temperature. |
| AnAge | Candidate source with caveats; units and mass-specific rates must be converted carefully. |
| FmrBT | FMR only; record but do not use for baseline ΔE computation. |

### Conflicting-source tie-break rule

If multiple candidate sources report conflicting metabolic-rate values for the same exact species and physiological state, do not average them silently. Prefer, in order:

1. peer-reviewed primary literature over secondary databases,
2. exact species over genus/sister-species proxy,
3. matching mass + metabolic-rate measurement from the same study over mixed-source mass/rate pairings,
4. correct physiological state and fasting/resting conditions over newer but confounded data,
5. more recent peer-reviewed source only when methodology is equal or better.

Log every conflict in `source_verification_matrix.csv` and `missing_data_log.md`, including the chosen source, rejected source(s), and the reason for the choice.

---

## Step 2 — Implement `source_verification_matrix.csv`

Required columns:

```csv
species_name,source_name,source_type,exact_species_match,contains_metabolic_rate,contains_body_mass,contains_temperature,contains_physiological_state,usable_for_delta_e,reason_not_usable,citation_or_url,notes
```

Rules:

- `usable_for_delta_e` must be false for sources that lack measurement state or temperature when those are required.
- FMR/MMR/active/postprandial/diving-only data may be logged but must not be used for baseline ΔE unless a human explicitly approves a proxy-only exploratory run.

---

## Step 3 — Implement `allometry_config.json`

No allometric constants may be hard-coded in Python.

Every baseline equation must live in `data/allometry_config.json` and include:

```json
{
  "clade": "Mammalia",
  "equation_id": "example_only_do_not_use_without_verification",
  "equation_type": "PGLS | OLS | species_specific | clade_specific",
  "a": null,
  "b": null,
  "mass_units": "g | kg",
  "metabolic_rate_units": "ml_O2_h | W | other",
  "log_space": true,
  "temperature_reference_c": null,
  "physiological_state": "BMR | SMR | RMR",
  "allowed_taxa": [],
  "citation": "",
  "verified": false,
  "notes": ""
}
```

A row with `verified: false` must not be used for computation.

---

## Step 4 — Implement `real_delta_e_candidates.csv`

Required columns are defined in `13_PHASE2_DATA_SCHEMA.md` and mirrored in `templates/real_delta_e_candidates_schema.csv`.

Minimum required fields for computing ΔE:

- exact species name
- mass paired to the same metabolic measurement
- metabolic rate value
- metabolic rate units
- physiological state
- measurement temperature where relevant
- selected allometry equation ID
- predicted metabolic rate
- signed log residual
- absolute magnitude
- source citation
- quality flags

---

## Step 5 — Compute only eligible rows

Eligible physiological states for baseline computation:

```text
BMR, SMR, RMR
```

States to record but not compute by default:

```text
FMR, MMR, AMR, DMR, active_swimming, postprandial, SDA, field, routine_unclear
```

Proxy/sister-species data:

- may be recorded,
- must trigger `FLAG_PROXY_SPECIES`,
- must leave `signed_log_residual` and `delta_E_magnitude` null unless a separate human-approved proxy mode is run.

---

## Step 6 — Temperature handling

For endotherms:

- Require thermoneutral-zone or suitable resting metadata.
- Flag possible cold stress, torpor, artificial selection, or agricultural strain issues.

For ectotherms:

- Require measurement temperature.
- Prefer source equations already normalized to a reference temperature.
- If applying Q10/Arrhenius correction, the correction equation must be explicitly configured and cited.
- If no defensible correction is available, leave ΔE null and log the gap.

Do not merely ignore temperature for ectotherm measurements.

---

## Step 7 — Missing-data log

Create `data/missing_data_log.md` with one section per missing or excluded species/measurement.

Required section fields:

```text
SPECIES:
FIELD:
STATUS:
Attempted sources:
Available alternative data:
Reason for exclusion:
Fallback used:
Human review needed:
Impact on Phase 3 readiness:
```

---

## Step 8 — Phase 3 readiness gate

A Phase 3 real-ΔE rerun is allowed only if all are true:

1. At least 3 of 5 target species have exact-species, eligible metabolic measurements.
2. Every computed row uses a verified allometry equation.
3. Every computed row has units converted and documented.
4. Every ectotherm row has temperature handled by verified equation or marked non-computable.
5. Proxy rows are not silently used.
6. `real_delta_e_candidates.csv` passes validation.
7. A human explicitly approves running Phase 3 with the available N.

If this gate fails, produce a data-gap report instead of running Phase 3. Compute-eligibility criteria must not be loosened, reinterpreted, or special-cased to reach the N≥3 threshold; report `INSUFFICIENT_COMPUTABLE_N` honestly if the gate fails on its own terms.

---

## Step 9 — Only after gate passes: Phase 3 real-input mode

Then and only then update `cne_simulation.py` to accept:

```bash
--delta-e-file data/real_delta_e_candidates.csv
--delta-e-column delta_E_magnitude
--label-column species_name
--real-delta-e
```

Outputs:

```text
outputs/phase3_real_delta_e_results.json
outputs/phase3_real_delta_e_sensitivity.json
outputs/phase3_real_delta_e_simulation.png
```

Do not tune constants to recover a sigmoid.

---

## Stop conditions

Stop and request review if:

- the selected ΔE proxy is being described as historical transition energy,
- more than 2 of 5 target species are non-computable,
- an agent appears to be loosening compute-eligibility criteria to reach N≥3,
- temperature cannot be handled for ectotherms,
- allometry constants are unverified,
- a proxy species is needed for a headline row,
- a result looks publishable but lacks sensitivity checks,
- any file proposes reviving the old thermodynamic-floor framing.
