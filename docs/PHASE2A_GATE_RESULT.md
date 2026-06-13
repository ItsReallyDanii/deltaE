# Phase 2A Gate Result — Real ΔE Grounding

## Summary

Phase 2A source verification and data-quality grounding has been completed. All four required deliverables were produced, independently audited, and patched. The final audit status is **PASS**. The Phase 3 readiness gate verdict is **INSUFFICIENT_COMPUTABLE_N**.

| Field | Value |
|---|---|
| Phase | 2A — Source Verification and Candidate ΔE Computation |
| Audit status | PASS (after four patches) |
| Gate verdict | INSUFFICIENT_COMPUTABLE_N |
| Target species compute-eligible | 2 of 5 (updated — see Addendum) |
| Gate threshold | 3 of 5 |
| Phase 3 authorized | NO |

---

## Produced Artifacts

| File | Description |
|---|---|
| `data/source_verification_matrix.csv` | 46 rows — all 7 species (5 target + 2 reference) crossed against AnAge, AnimalTraits, AmphiBIO, EltonTraits, TimeTree, and primary literature sources. `usable_for_delta_e` and `reason_not_usable` populated for every row. |
| `data/allometry_config.json` | 5 allometric equations: `REP_BD76` (Bennett & Dawson 1976 reptile SMR at 30°C), `CP_SEY13` (Seymour 2013 *C. porosus* species-specific), `MAM_KLB47` (Kleiber 1947 mammal BMR), `AVE_MW04` (McKechnie & Wolf 2004 avian BMR), `TUNN_CLK08_PROXY` (Clark 2008 *T. maccoyii*, `verified=false`). No constants hard-coded in Python. |
| `data/real_delta_e_candidates.csv` | 7 rows, full 26-column schema. `signed_log_residual` and `delta_E_magnitude` populated only for `compute_eligible=TRUE` rows. |
| `data/missing_data_log.md` | Per-species sections for all 4 non-computable target species, with primary blockers, resolution paths, and Phase 3 impact assessment. |

---

## Audit Patch Record

Four issues were identified during the independent audit and corrected before the gate verdict was finalised.

| # | Location | Issue | Fix |
|---|---|---|---|
| 1 | `MM001 mass_measured_g` | Field contained `26500` (= 26.5 kg). *Mus musculus* weighs ~26.5 g. Residual computation was correct (notes used 0.0265 kg); only the CSV field was wrong. | Corrected to `26.5` |
| 2 | `AM001 notes` | Arithmetic error: `0.188 × 1079^0.80` was written as `0.188 × 214.4 = 40.3`. Correct intermediate is 266.9, giving 50.2 ml O₂/h. Non-compute conclusion and temperature-mismatch rationale were unaffected. | Corrected to `0.188*266.9 = 50.2 ml O2/h` |
| 3 | `CP001 quality_flags` | `FLAG_MASS_NOT_PAIRED_TO_RATE` absent. The 100 kg representative mass is a modeling choice from the Seymour 2013 equation; no single animal provides a directly paired measurement. | Added `FLAG_MASS_NOT_PAIRED_TO_RATE` |
| 4 | `GG001 quality_flags` | `FLAG_TNZ_UNCLEAR` absent. AnAge does not specify measurement temperature; thermoneutral conditions are assumed (`ENDOTHERM_TNZ_ASSUMED`), not confirmed. MM001 already carried this flag; GG001 was inconsistent. | Added `FLAG_TNZ_UNCLEAR` |

All computed residuals (`CP001`, `GG001`, `MM001`) were verified arithmetically correct before and after patching.

---

## Compute-Eligibility Summary

| Species | Role | Compute-Eligible | δE magnitude | Primary blocker / Notes |
|---|---|---|---|---|
| *Crocodylus porosus* | **Target** | **YES** | **0.1664** | Provisional / equation-derived. Seymour et al. 2013 species-specific SMR equation at 30°C used at representative mass 100 kg. Baseline: REP_BD76 (Bennett & Dawson 1976). `FLAG_MASS_NOT_PAIRED_TO_RATE` — no single paired animal measurement; representative mass is a modeling choice within the study range. |
| *Alligator mississippiensis* | **Target** | **YES** (updated) | **0.1254** | **RESOLVED** via Gienger et al. 2017 (PLOS ONE 12(2):e0171082). Exact-species SMR at 30°C, fasted 3–4 days, n=7, mass range 1.29–104 kg. Equation AM_GBT17: SMR = 0.491 × M_kg^0.965 ml O₂/min. Representative mass 100 kg used. Baseline: REP_BD76. `FLAG_MASS_NOT_PAIRED_TO_RATE` applied (equation-derived, not single paired animal). See Addendum. |
| *Heterocephalus glaber* | **Target** | NO | null | Data exists (O'Connor 1999; ~13.8 ml O₂/h at 30°C for ~35 g). Not computed due to thermoconforming physiology: *H. glaber* metabolic rate tracks ambient temperature; the standard mammalian thermoneutral-zone (TNZ) concept does not apply. Using the Kleiber mammal baseline would produce a large negative residual reflecting thermoconforming biology rather than clade-relative evolutionary deviation. **Recovery path:** human method decision required — either (a) accept measurement with `FLAG_TNZ_UNCLEAR` and explicit interpretation caveat, or (b) classify as permanently non-computable. |
| *Dermochelys coriacea* | **Target** | NO | null | No whole-animal BMR or SMR exists in the literature. Only diving metabolic rate (DMR ≈ 0.73 ml O₂/kg/min, Southwood 2007) is available; DMR is an explicitly excluded physiological state. Maintaining elevated core temperature via gigantothermy, not elevated basal metabolic rate. Technically infeasible to measure whole-animal BMR for a 350+ kg obligate marine animal. **Structurally absent — not resolvable from existing data.** |
| *Thunnus thynnus* | **Target** | NO | null | No direct *T. thynnus* standardized metabolic rate exists. Available proxy: *T. maccoyii* RMR = 460 mg O₂/kg/h at 19°C (Clark 2008). Four simultaneous disqualifiers: `FLAG_PROXY_SPECIES`, `FLAG_NON_STANDARD_STATE` (obligate ram ventilators cannot rest), `FLAG_TEMP_UNCORRECTED_REVIEW_REQUIRED` (19°C, no authorized correction), and no verified clade-level allometric baseline for Thunnini in `allometry_config.json`. **Structurally absent — not resolvable from existing data without new primary measurements.** |
| *Gallus gallus* | **Reference only** | YES (reference) | 0.8279 | **Not counted toward target N.** Outgroup species for Archosauria. Large positive residual (δE = 0.828) likely attributable to artificial selection in domestic strain; `FLAG_ARTIFICIAL_SELECTION_RISK` and `FLAG_TNZ_UNCLEAR` applied. Human review recommended before using this value in comparative analysis. |
| *Mus musculus* | **Reference only** | YES (reference) | 0.1836 | **Not counted toward target N.** Outgroup species for Mammalia. Selman et al. 2001 C57BL/6 RMR at 23°C. `FLAG_TNZ_UNCLEAR` (measurement temperature slightly below murine TNZ ~26–34°C). |

---

## Explicit Statements

**Phase 3 must not run.** The Phase 3 readiness gate has failed (2 of 5 target species compute-eligible after Alligator source recovery; threshold is 3 of 5). No update to `cne_simulation.py`, no real-ΔE rerun, and no new Phase 3 outputs are authorized under the v2.1 control package rules until the gate passes with human explicit approval.

**The placeholder Phase 3 result remains artifact/provenance only.** The previously reported sigmoid threshold (δE ≈ 0.554) and lock-in ratio (253,988×) were produced using placeholder ΔE values [0.1, 0.25, 0.5, 0.75, 1.0] with no biological grounding. They remain on record for methodological provenance only and must not be cited as findings.

**The current species set is not data-sufficient for the claimed ΔE test.** With only 2 of 5 target species yielding compute-eligible residuals — both rows marked provisional/equation-derived — any Phase 3 output using this data would have insufficient statistical power and would not constitute an empirical test of the EME hypothesis.

---

## Next Allowed Actions

1. ~~**Narrow source recovery for *Alligator mississippiensis*:**~~ **COMPLETED** — Gienger et al. 2017 identified; AM001 is now compute-eligible (δE = 0.1254). See Addendum.

2. **Human method decision for *Heterocephalus glaber*:** The project owner must decide whether to accept the O'Connor (1999) thermoconformer measurement as compute-eligible with full flagging, or classify it as permanently non-computable. Record the decision and rationale in `data/missing_data_log.md`.

3. **Optional Phase 2B — only if explicitly authorized:** If the project owner decides to expand the species list beyond the current five targets, create a separate recommendation file before adding any species. Do not silently substitute sister taxa.

---

## Forbidden Next Actions

- **Running Phase 3** in any form (real-ΔE or placeholder) without explicit human approval after the gate passes.
- **Claim-registry upgrade** — no README or control-package claim may be updated to "supported" or "confirmed" based on current Phase 2A outputs.
- **Proxy substitution** — no sister species may be silently used as a compute-eligible data source for any target species.
- **Eligibility-gate gaming** — compute-eligibility criteria must not be loosened, reinterpreted, or special-cased to reach N ≥ 3.
- **Reviving thermodynamic-floor wording** — the metric is Current Clade-Relative Metabolic Deviation; ancestral-transition-energy framing is not supported by the available data.
- **Claiming the hypothesis passed** — a valid clean gate fail at N = 2 is the correct and honest outcome.

---

## Addendum — Alligator mississippiensis Source Recovery

**Date of patch:** Post-initial gate result  
**Patch type:** Source recovery — no eligibility criteria loosened

A primary-literature source was identified that resolves the Alligator mississippiensis temperature-mismatch blocker without requiring a Q10 correction or any proxy species.

**Source:** Gienger CM, Brien ML, Tracy CR, Manolis SC, Webb GJW, Seymour RS & Christian KA (2017). Ontogenetic comparisons of standard metabolism in three species of crocodilians. PLOS ONE 12(2):e0171082. doi:10.1371/journal.pone.0171082. PMC5300253.

**Key findings used:**
- Exact species: *Alligator mississippiensis*
- Physiological state: SMR, fasted 3–4 days (post-absorptive) ← compute-eligible state
- Measurement temperature: 30°C ← matches REP_BD76 reference; no correction needed
- Allometric equation: SMR = 0.491 × M_kg^0.965 ml O₂/min (entry AM_GBT17 in `allometry_config.json`, `verified=true`)
- Study mass range: 1.29–104 kg; representative mass 100 kg used (matching CP001 approach)

**Computed values:**
- Measured SMR at 100 kg: 0.491 × 100^0.965 = 41.80 ml O₂/min = 2508.1 ml O₂/h
- REP_BD76 baseline at 100,000 g: 0.188 × 100000^0.80 = 1880.0 ml O₂/h
- signed_log_residual: log10(2508.1/1880.0) = 0.1254
- delta_E_magnitude: 0.1254
- Quality flags: `FLAG_COMPUTE_ELIGIBLE;FLAG_MASS_NOT_PAIRED_TO_RATE`

**Files updated by this patch:**
- `data/real_delta_e_candidates.csv` — AM001 row replaced; now compute_eligible=TRUE
- `data/source_verification_matrix.csv` — Gienger et al. 2017 row added (usable_for_delta_e=TRUE)
- `data/allometry_config.json` — AM_GBT17 equation added (verified=true)
- `data/missing_data_log.md` — Alligator section updated to RESOLVED; summary table updated

**Updated gate state:**

| Field | Original | After Alligator patch |
|---|---|---|
| Audit status | PASS | PASS |
| Target species compute-eligible | 1 of 5 | **2 of 5** |
| Gate threshold | 3 of 5 | 3 of 5 |
| Gate verdict | INSUFFICIENT_COMPUTABLE_N | **INSUFFICIENT_COMPUTABLE_N** |
| Phase 3 authorized | NO | NO |

Gate still fails. *Heterocephalus glaber* remains the only remaining plausible path to N=3, subject to a separate human method decision on thermoconforming physiology (see `data/missing_data_log.md`). *Dermochelys coriacea* and *Thunnus thynnus* remain structurally non-computable.
