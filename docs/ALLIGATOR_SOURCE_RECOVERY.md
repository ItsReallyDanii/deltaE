# Alligator mississippiensis — Phase 2A Source Recovery Investigation

## Scope

Narrow source-recovery investigation for *Alligator mississippiensis* only, per the Phase 2A gate's "Next Allowed Actions" item 1. This document does not modify any data files, does not touch Phase 3, does not change H. glaber, and does not alter compute-eligibility criteria.

---

## 1. Executive Verdict

**`SOURCE_FOUND_COMPUTE_ELIGIBLE`**

A primary-literature, exact-species, fasted, standard-metabolic-rate (SMR) measurement series at 30°C exists for *A. mississippiensis*: Gienger et al. (2017), *PLOS ONE* 12(2):e0171082, doi:10.1371/journal.pone.0171082 (PMC5300253). This source is structurally equivalent to the Seymour et al. (2013) source already accepted for *Crocodylus porosus* (CP001) under the existing package criteria — a verified species-specific allometric SMR equation, measured at the REP_BD76 reference temperature (30°C), fasted, with a documented mass range.

---

## 2. Candidate Source Table

| Citation | Exact species? | Physiological state | Fasting/post-absorptive? | Temperature | Body mass paired to rate? | Metabolic rate value | Units | Conversion path to ml O₂/h | Eligibility verdict | Reason if not eligible |
|---|---|---|---|---|---|---|---|---|---|---|
| **Gienger CM, Brien ML, Tracy CR, Manolis SC, Webb GJW, Seymour RS, Christian KA (2017) Ontogenetic comparisons of standard metabolism in three species of crocodilians. PLOS ONE 12(2):e0171082. doi:10.1371/journal.pone.0171082. PMC5300253.** | TRUE — *A. mississippiensis* is one of the three species measured directly (n=7 individuals, 1.29–104 kg) | SMR (standard metabolic rate; lowest recorded O₂ consumption during multi-day trial) | YES — animals fasted 3–4 days before respirometry, measured after 6–12 h settling period | 30°C (metabolic chambers actively heated and held at 30°C) | YES, as a fitted allometric equation over 7 individuals spanning 1.29–104 kg (same structure as the CP_SEY13 equation already used for CP001) | SMR = 0.491 · M(kg)^0.965 (ml O₂/min); least-square mean SMR = 2.77 ml O₂/min (s.e. = 0.077) across the sampled size range | ml O₂/min, M in kg | ml O₂/min × 60 = ml O₂/h. At representative mass 100 kg (same representative mass used for CP001, and within the 1.29–104 kg study range): SMR = 0.491 × 100^0.965 = 0.491 × 85.114 = 41.80 ml O₂/min = **2508.1 ml O₂/h** | **ELIGIBLE** (equation-derived, representative-mass approach — same modeling choice already accepted for CP001) | — |
| Lewis & Gatten (1985) Comp Biochem Physiol A 80:441-447. PMID 2858324. doi:10.1016/0300-9629(85)90065-9. | TRUE | SMR ("standard metabolic rate," dry chamber and water-tank, 10–35°C range, lowest steady-state value used) | YES — abstract explicitly states SMR did not vary with "duration of fasting before measurement," confirming fasted/post-absorptive protocol | Measured across 10–35°C; 30°C value not separately tabulated in available abstract/metadata (no PMC full text; ScienceDirect paywalled) | Reported in mass-specific units only; mean body mass 1.29 kg (per Gienger et al. 2017, which reanalyzed this same dataset) | Not independently extractable — abstract gives no numeric SMR value at 30°C; only Gienger et al. (2017)'s reuse of these 4 juveniles (folded into the n=7 dataset above) provides a usable number | mass-specific (ml O₂ g⁻¹ h⁻¹ or similar, units not confirmed without full text) | Not computable independently — would require full-text access | **NOT INDEPENDENTLY ELIGIBLE** (subsumed into Gienger et al. 2017 dataset above) | No numeric 30°C value retrievable from abstract/metadata; full text behind Elsevier paywall. Its data are already incorporated into the Gienger et al. (2017) equation, so it does not provide an independent additional data point. |
| Coulson & Hernandez (1983) Comp Biochem Physiol 74A:1-182 (monograph). | TRUE | Multiple (extensive metabolic study) | Unconfirmed | Unconfirmed at 30°C | Unconfirmed | Not retrievable — pre-1985 monograph, not in PubMed/PMC, no DOI found via PubMed search | Unconfirmed | Unconfirmed | **NOT EVALUATED — NO ACCESS** | Could not be located via PubMed, PMC, or web search within this run; same status as recorded in the existing source_verification_matrix.csv (full-text access required, not obtained). No change from prior status. |
| AnAge database entry for *A. mississippiensis* | TRUE | RMR | Not specified | 22.2°C | Yes (1079 g) | 0.1539 W = 27.56 ml O₂/h | W → ml O₂/h via 179.1 ml O₂/h/W | Already documented (AM001) | **NOT ELIGIBLE** (no change — already in CSV as non-eligible) | Temperature mismatch (22.2°C vs. 30°C reference); unchanged from current state. Listed here only for completeness, not as a new finding. |

---

## 3. Best Candidate Source

**Gienger et al. (2017), PLOS ONE 12(2):e0171082**, is the best — and only newly viable — candidate.

- **Exact species match**: yes, direct *A. mississippiensis* measurements (n=7), not a proxy.
- **Physiological state**: SMR, the same standard the package requires (matches REP_BD76's SMR basis and CP001's SMR basis).
- **Fasting**: confirmed — protocol fasted animals 3–4 days pre-measurement.
- **Temperature**: 30°C — exactly matches the REP_BD76 reference temperature. **No Q10/Arrhenius correction is needed**, because this source removes the temperature mismatch entirely rather than correcting for it.
- **Mass pairing**: the equation `SMR = 0.491·M^0.965` is fitted to 7 individuals spanning 1.29–104 kg (including the Lewis & Gatten 1985 juveniles, reanalyzed). This is the same "fitted species-specific allometric equation, evaluated at a representative mass" structure already accepted for CP001 via the Seymour et al. (2013) / CP_SEY13 equation.

**Can it replace or patch the current AM001 row?**
Yes. It can **patch** AM001 (not the AnAge row identity, but the source, measurement, and computation fields) following the exact precedent set by CP001. The AnAge 22.2°C measurement remains documented as historical/superseded context (as Phase 2A already does for provenance), but the row's `compute_eligible`, `source_name`, `source_citation`, and computation fields would be updated to reflect the Gienger et al. (2017) source.

**Does it reach compute eligibility under existing criteria?**
Yes, under the same modeling-choice logic already used and flagged for CP001:
- Equation source is verified primary literature (peer-reviewed, PLOS ONE, open access, PMC5300253).
- Measurement state (SMR), fasting status, and temperature (30°C) all match REP_BD76's requirements exactly — no temperature correction needed.
- The representative-mass approach (100 kg) mirrors CP001 exactly, carrying the same `FLAG_MASS_NOT_PAIRED_TO_RATE`-type caveat (the equation is fit to a population, evaluated at a chosen mass within range, not a single paired animal-rate measurement).

Worked computation at representative mass = 100 kg (within the 1.29–104 kg study range, and matching the mass used for CP001 for direct comparability):

```
SMR(measured, Gienger 2017) = 0.491 * 100^0.965 ml O2/min
                             = 0.491 * 85.114
                             = 41.80 ml O2/min
                             = 41.80 * 60 = 2508.1 ml O2/h

SMR(baseline, REP_BD76) at 100,000 g = 0.188 * 100000^0.80
                                       = 0.188 * 10000
                                       = 1880 ml O2/h

signed_log_residual = log10(2508.1 / 1880) = log10(1.3341) = 0.1254
delta_E_magnitude   = 0.1254
```

For reference, CP001 (C. porosus, same baseline, same representative mass) has signed_log_residual = 0.1664. The new A. mississippiensis value (0.1254) is lower than C. porosus's, consistent with Gienger et al. (2017)'s finding that *A. mississippiensis* SMR is below *C. porosus* SMR at comparable body sizes — a qualitative cross-check that the computed value is directionally sensible.

---

## 4. Required Patch Plan (if source is used) — NOT APPLIED IN THIS RUN

This section describes the patch that *would* be required. **No data files have been edited in this run.**

### 4.1 `data/real_delta_e_candidates.csv` — AM001 row fields to update

| Field | Current value | Proposed new value |
|---|---|---|
| `source_name` | AnAge database | Gienger et al. 2017 |
| `source_citation` | AnAge: ... | Gienger CM, Brien ML, Tracy CR, Manolis SC, Webb GJW, Seymour RS, Christian KA (2017) Ontogenetic comparisons of standard metabolism in three species of crocodilians. PLOS ONE 12(2):e0171082. doi:10.1371/journal.pone.0171082. PMC5300253. |
| `exact_species_match` | TRUE | TRUE (unchanged) |
| `proxy_species_used` | FALSE | FALSE (unchanged) |
| `mass_measured_g` | 1079 | 100000 (representative mass, mirrors CP001's representative-mass approach) |
| `mass_source` | AnAge database | Representative mass (100 kg) chosen within the 1.29–104 kg range of Gienger et al. (2017), for direct comparability with CP001's representative mass |
| `metabolic_rate_measured` | 0.1539 | 41.80 |
| `metabolic_rate_units` | W | ml_O2_min |
| `metabolic_rate_standardized_value` | 27.56 | 2508.1 |
| `metabolic_rate_standardized_units` | ml_O2_h | ml_O2_h (unchanged) |
| `physiological_state` | RMR | SMR |
| `temp_measurement_c` | 22.2 | 30.0 |
| `temp_normalized_c` | (blank) | 30.0 |
| `temperature_handling_method` | NOT_COMPUTED_TEMPERATURE_MISMATCH | measurement_at_reference_temperature_no_correction_needed |
| `allometry_equation_id` | REP_BD76 | REP_BD76 (unchanged — still the clade baseline) |
| `predicted_metabolic_rate` | (blank) | 1880.0 |
| `predicted_metabolic_rate_units` | ml_O2_h | ml_O2_h (unchanged) |
| `signed_log_residual` | (blank) | 0.1254 |
| `delta_E_magnitude` | (blank) | 0.1254 |
| `compute_eligible` | FALSE | TRUE |
| `quality_flags` | FLAG_NOT_COMPUTE_ELIGIBLE;FLAG_TEMP_UNCORRECTED_REVIEW_REQUIRED | FLAG_COMPUTE_ELIGIBLE;FLAG_MASS_NOT_PAIRED_TO_RATE |
| `notes` | (existing AnAge note) | Updated note explaining the Gienger et al. (2017) equation, the 100 kg representative mass choice, the computation shown above, and retaining the superseded AnAge 22.2°C value for provenance (as CP001's notes retain its own caveats). |

A new equation entry analogous to `CP_SEY13` (e.g., `AM_GBT17`) would also need to be added to `allometry_config.json` (see 4.4) to document the measured-value equation, mirroring how `CP_SEY13` documents Seymour et al. (2013) for CP001.

### 4.2 `data/source_verification_matrix.csv` — rows to add/update

- **Update** the existing Coulson & Hernandez (1983) row's `notes`/`reason_not_usable` only if full-text access is later obtained (no change proposed here; status unchanged).
- **Add** a new row:

| species_name | source_name | source_type | exact_species_match | contains_metabolic_rate | contains_body_mass | contains_temperature | contains_physiological_state | usable_for_delta_e | reason_not_usable | citation_or_url | notes |
|---|---|---|---|---|---|---|---|---|---|---|---|
| Alligator mississippiensis | Gienger et al. 2017 | primary_literature | TRUE | TRUE | TRUE | TRUE | TRUE | TRUE | (blank — usable) | https://doi.org/10.1371/journal.pone.0171082 (PMC5300253) | SMR = 0.491*M_kg^0.965 ml O2/min at 30°C, fasted 3-4 days, n=7 (1.29-104 kg), incorporates Lewis & Gatten (1985) juveniles. Matches REP_BD76 reference temperature exactly; no Q10 correction required. |

### 4.3 `data/missing_data_log.md` — notes needed

- Update the *Alligator mississippiensis* section: change status from "DATA EXISTS AT WRONG TEMPERATURE" to "RESOLVED — primary-literature 30°C SMR equation located (Gienger et al. 2017)."
- Retain the AnAge 22.2°C value and the temperature-mismatch discussion as historical/provenance context (do not delete — it documents why the original AM001 entry was non-eligible and shows the reasoning trail).
- Note explicitly that the new value uses a **representative-mass, equation-derived** approach (same caveat class as CP001, `FLAG_MASS_NOT_PAIRED_TO_RATE`), not a single paired animal measurement — so the same caveat language used for CP001 should be mirrored for AM001.
- Update the "Impact on Phase 3 readiness" note: resolving AM001 brings target N from 1 to 2 (still below threshold of 3; H. glaber human decision remains the only path to N=3).

### 4.4 `data/allometry_config.json` — changes needed

Two possible approaches; a human should confirm which is preferred before any edit is made:

1. **Mirror CP_SEY13 exactly** (recommended for consistency): add a new entry `AM_GBT17` documenting `SMR_ml_O2_min = 0.491 * M_kg^0.965` for *Alligator mississippiensis* at 30°C, `verified: true`, citing Gienger et al. (2017), with a `notes` field stating this is the *measured* value equation (analogous role to CP_SEY13), while `REP_BD76` remains the *baseline* equation (avoiding circularity, exactly as CP001's notes already explain for CP_SEY13 vs REP_BD76).
2. Alternatively, since the measured value can be computed directly and entered as a literal number in `real_delta_e_candidates.csv` (as done above), the config addition is documentation/provenance rather than strictly required for computation — but adding it preserves the "no constants hard-coded in Python" principle already established for CP001.

**No change to `REP_BD76`, `MAM_KLB47`, `AVE_MW04`, or `TUNN_CLK08_PROXY` is proposed.** No Q10/Arrhenius equation is added or needed — the recovered source already matches the 30°C reference temperature.

---

## 5. Gate Impact

| Item | Value |
|---|---|
| Current target compute-eligible count | 1 / 5 (gate threshold 3/5) — **INSUFFICIENT_COMPUTABLE_N** |
| Count if Alligator becomes eligible (this patch applied) | 2 / 5 |
| Is N ≥ 3 reached? | **No.** 2/5 is still below the threshold of 3/5. Gate verdict would remain INSUFFICIENT_COMPUTABLE_N even after this patch. |
| Heterocephalus glaber status | **Unchanged and unaddressed by this investigation.** *H. glaber* would still require a separate human method decision (accept thermoconformer BMR with `FLAG_TNZ_UNCLEAR`, or classify as permanently non-computable) before it could count toward N. Only if that decision is "accept" AND this Alligator patch is applied would N reach 3/5 — exactly at threshold, still requiring human approval per the gate's existing rules. |
| Dermochelys coriacea / Thunnus thynnus | Unchanged — both remain structurally non-computable per existing findings; not addressed by this investigation (out of scope). |

---

## 6. Do-Not-Proceed Statement

**Phase 3 remains blocked.** Even if the patch plan in Section 4 is applied, the resulting target compute-eligible count (2/5) does not reach the gate threshold (3/5), so the gate verdict would remain **INSUFFICIENT_COMPUTABLE_N** and **Phase 3 must not run**. Reaching N≥3 additionally requires a human method decision on *Heterocephalus glaber* (Section 5). Even at N=3 exactly at threshold, explicit human approval is required before Phase 3 may run, per the existing control package rules. No data files (`real_delta_e_candidates.csv`, `source_verification_matrix.csv`, `missing_data_log.md`, `allometry_config.json`), `cne_simulation.py`, or claim-registry status have been modified in this run.
