# Missing Data Log — Phase 2A

**Project:** EME Phase 2 — Real ΔE (Delta E) Grounding  
**Metric:** Current Clade-Relative Metabolic Deviation  
**Formula:** `signed_log_residual = log10(metabolic_rate_standardized / predicted_metabolic_rate)`  
**Generated:** Phase 2A source verification pass  
**Status:** 2 of 5 target species compute-eligible after Alligator source recovery; gate INSUFFICIENT_COMPUTABLE_N

---

## Dermochelys coriacea

- **Field missing:** Whole-animal BMR or SMR (any standardized resting metabolic rate)
- **Status:** NOT AVAILABLE — structurally absent; technically infeasible to obtain
- **Attempted sources:**
  - AnAge database: no metabolic rate field for D. coriacea
  - AnimalTraits database: no SMR entry confirmed for large marine reptile
  - Southwood et al. 2007 (J Exp Biol 210:1914-1924): provides diving metabolic rate (DMR) only (~0.73 ml O₂/kg/min inferred from behavioral time-activity budgets); DMR ≠ BMR/SMR
  - Wallace et al. 2005 (J Exp Mar Biol Ecol 318:73-86): field metabolic rate from heart-rate monitoring; not a controlled resting measurement
  - Paladino et al. 1990 (Nature 344:858-860): tissue-level thermal independence studies; no whole-animal rate
- **Available alternative data:**
  - Diving metabolic rate (DMR): ~0.73 ml O₂/kg/min (Southwood 2007) — non-compute physiological state
  - Core temperature maintenance demonstrated via gigantothermy, not elevated BMR
  - Body mass (adult female): ~350 kg
- **Reason for exclusion:** D. coriacea is a 350+ kg obligate marine animal. Measuring a true whole-animal BMR requires post-absorptive, thermoneutral resting conditions in a closed respirometry system. These conditions are technically infeasible for a large marine reptile that must remain in water and cannot sustain complete inactivity. All published metabolic measurements are field or behavioral estimates, not controlled resting rates. DMR is explicitly non-compute per the project schema.
- **Fallback used:** None. DMR documented in real_delta_e_candidates.csv with `compute_eligible=FALSE` and `FLAG_NON_STANDARD_STATE`.
- **Human review needed:** No additional review changes the data availability. D. coriacea should be formally documented as permanently non-computable unless a future whole-animal respirometry study becomes technically feasible (e.g., juvenile animals in controlled aquarium setting).
- **Impact on Phase 3 readiness:** Reduces maximum compute-eligible target species from 5 to 4. Combined with Thunnus thynnus exclusion, reduces maximum possible N to 3 — exactly at the gate threshold. Any additional exclusion fails the gate regardless of other species.

---

## Thunnus thynnus

- **Field missing:** Direct Thunnus thynnus whole-animal standardized metabolic rate (BMR, SMR, or RMR at defined reference temperature)
- **Status:** PROXY ONLY — no T. thynnus specific basal rate; sister species and wrong physiological state
- **Attempted sources:**
  - Clark et al. 2008 (J Fish Biol 72:1289-1298, PMID 17081787): T. maccoyii (southern bluefin tuna) RMR = 460 mg O₂/kg/h at 19°C in tunnel respirometer — proxy species, wrong physiological state
  - Fitzgibbon et al. 2010 (J Exp Biol 213:2379-2385): Pacific bluefin (T. orientalis) postprandial data only — wrong species AND wrong physiological state (postprandial is excluded)
  - Korsmeyer & Dewar 2001 (Fish Physiol Biochem 25:3-24): review of tuna active metabolic rates — active/swimming rates, not resting
  - AnAge database: no metabolic rate field for T. thynnus
  - AnimalTraits database: no SMR entry confirmed for T. thynnus
  - FishBase: no BMR field for T. thynnus
- **Available alternative data:**
  - Sister species proxy (T. maccoyii): RMR 460 mg O₂/kg/h at 19°C (Clark 2008) — disqualified (FLAG_PROXY_SPECIES + FLAG_NON_STANDARD_STATE + FLAG_TEMP_UNCORRECTED_REVIEW_REQUIRED)
  - T. thynnus known to be regionally endothermic (elevated muscle and visceral temperatures) but whole-animal basal rate unknown
  - Body mass (adult): ~150-400 kg
- **Reason for exclusion:** Tuna are obligate ram ventilators — they must swim continuously to ventilate gills. A true post-absorptive, resting metabolic rate is biologically infeasible to measure. All available measurements are either minimum sustained swimming rates (RMR in tunnel respirometer), field metabolic rates, or postprandial rates. No direct T. thynnus measurement exists; closest available is a sister-species RMR at a non-standard temperature. All four disqualifiers apply simultaneously: proxy species, wrong physiological state, wrong temperature, no verified allometric baseline equation for Thunnini.
- **Fallback used:** T. maccoyii proxy data documented in real_delta_e_candidates.csv with `compute_eligible=FALSE`, `proxy_species_used=TRUE`, `FLAG_PROXY_SPECIES`, `FLAG_NON_STANDARD_STATE`, and `FLAG_TEMP_UNCORRECTED_REVIEW_REQUIRED`.
- **Human review needed:** No single-step resolution exists. A future compute-eligible T. thynnus data point would require either: (a) primary literature measuring a standardized resting rate for Atlantic bluefin tuna under defined temperature conditions (technically very difficult), or (b) explicit human authorization of a sister-species proxy run flagged as exploratory.
- **Impact on Phase 3 readiness:** Combined with D. coriacea exclusion, reduces maximum possible target species N to 3. Both T. thynnus and D. coriacea are non-computable due to structural data unavailability, not insufficient search effort.

---

## Alligator mississippiensis

- **Field missing:** ~~Standardized metabolic rate measurement at the reference temperature (30°C)~~ — **RESOLVED**
- **Status:** RESOLVED via source recovery — Gienger et al. 2017 provides exact-species A. mississippiensis SMR at 30°C. AM001 is now compute-eligible. Target N rises from 1/5 to 2/5. Gate verdict remains INSUFFICIENT_COMPUTABLE_N.
- **Resolution source:** Gienger CM et al. (2017) Ontogenetic comparisons of standard metabolism in three species of crocodilians. PLOS ONE 12(2):e0171082. doi:10.1371/journal.pone.0171082. PMC5300253.
  - Exact species: *Alligator mississippiensis*
  - Physiological state: SMR (fasted 3–4 days, post-absorptive)
  - Temperature: 30°C — matches REP_BD76 reference; no Q10 correction needed
  - Equation: SMR = 0.491 × M_kg^0.965 ml O₂/min (allometry_config.json entry: AM_GBT17, `verified=true`)
  - Study mass range: 1.29–104 kg; representative mass 100 kg used (matching CP001 approach)
  - Computed δE: signed_log_residual = log10(2508.1/1880.0) = 0.1254; delta_E_magnitude = 0.1254
- **Provenance — prior non-computable record (retained for audit trail):**
  - AnAge database: resting MR = 0.1539 W at 1079 g at 22.2°C — not used for computation
  - Temperature mismatch (22.2°C vs 30°C REP_BD76 reference) was the original blocker; no Q10 correction was authorized
  - REP_BD76 at 1079 g correctly predicts 0.188 × 1079^0.80 = 0.188 × 266.9 = 50.2 ml O₂/h at 30°C; the AnAge measured value of 27.56 ml O₂/h at 22.2°C reflects the temperature depression, not a negative clade deviation
  - That AnAge row remains in source_verification_matrix.csv with `usable_for_delta_e=FALSE` for provenance
- **Fallback used:** None — resolved by primary literature source without proxy or Q10 correction.
- **Human review needed:** No — recovery is complete. AM001 is compute-eligible with standard flags.
- **Impact on Phase 3 readiness:** A. mississippiensis is now compute-eligible. Target N = 2 of 5. Gate threshold remains 3 of 5. Gate still fails. *Heterocephalus glaber* is the only remaining plausible path to N=3, requiring a separate human method decision on thermoconforming physiology (see H. glaber section below).

---

## Heterocephalus glaber

- **Field missing:** Human method decision on whether thermoconforming mammalian BMR is comparable to standard endotherm BMR for the purposes of delta_E computation
- **Status:** DATA EXISTS — physiological state interpretation requires human decision; compute_eligible is blocked pending that decision
- **Attempted sources:**
  - O'Connor TP (1999) Comp Biochem Physiol A 124:147-152 (PMID 10357434): primary BMR study; measured ~0.23 ml O₂/min at ~30°C for ~35 g animal
  - Lovegrove (2003) comparative BMR compilation: H. glaber cited as >40% below size-matched rodent BMR
  - AnAge: H. glaber entry may be present but temperature metadata not confirmed as usable
- **Available alternative data:**
  - O'Connor 1999: BMR ~0.23 ml O₂/min = 13.8 ml O₂/h at 30°C ambient for ~35 g animal
  - Established finding: H. glaber BMR is >40% lower than size-matched rodent prediction
  - Mass: ~30-80 g (laboratory colony range); representative 35 g used
- **Reason for exclusion (pending decision):** H. glaber is an atypical mammal — a thermoconformer. Its metabolic rate tracks ambient temperature rather than being regulated at a thermoneutral setpoint. The standard mammalian BMR concept (post-absorptive, thermoneutral, resting) does not apply in the usual sense. The measured rate (13.8 ml O₂/h at 30°C) would be compared to the Kleiber mammal baseline (MAM_KLB47, predicts 49.1 ml O₂/h at 35 g), yielding a hypothetical residual of log10(13.8/49.1) = −0.552. This large negative deviation likely reflects thermoconforming biology (metabolic rate tracking ambient temperature), not evolutionary "energetic surprise" in the sense the EME metric intends.
- **Fallback used:** Data documented in real_delta_e_candidates.csv with `compute_eligible=FALSE` and `FLAG_TNZ_UNCLEAR`. Hypothetical residual (−0.552) is NOT populated in the CSV.
- **Human review needed:** Yes — binary decision required: (a) **Accept as compute-eligible** with FLAG_TNZ_UNCLEAR and explicit documentation that the large negative residual reflects thermoconforming physiology; OR (b) **Classify as permanently non-computable** because thermoconforming physiology makes the mammalian BMR baseline inapplicable. This is a method decision, not a data-access problem. The raw data exists; the question is whether it is scientifically appropriate to compare it to the Kleiber clade baseline. Decision must be logged with rationale before H. glaber can be assigned `compute_eligible=TRUE`.
- **Impact on Phase 3 readiness:** If human decision is (a) — accept with flags — H. glaber becomes compute-eligible, giving N=2 target species (with C. porosus). Gate still fails at N=2. If A. mississippiensis is also resolved, N=3 and the gate passes with N exactly at threshold. If decision is (b) — reject — H. glaber remains non-computable regardless and the gate fails at N=1.

---

## Summary Table

| Species | Compute-Eligible | Primary Blocker | Resolvable? |
|---|---|---|---|
| Crocodylus porosus | **TRUE** | — | N/A |
| Alligator mississippiensis | **TRUE** | ~~Temperature mismatch~~ — **RESOLVED** via Gienger et al. 2017 | N/A — resolved |
| Heterocephalus glaber | FALSE | Thermoconforming physiology — human method decision | Yes — pending human decision |
| Dermochelys coriacea | FALSE | No whole-animal BMR exists; technically infeasible | No — structurally absent |
| Thunnus thynnus | FALSE | No direct species rate; proxy + non-standard state + wrong temp + no baseline equation | No — structurally absent |
| Gallus gallus (reference) | TRUE | — | N/A |
| Mus musculus (reference) | TRUE | — | N/A |

**Target species compute-eligible: 2 of 5** *(updated from 1/5 after Alligator source recovery)*  
**Gate threshold: 3 of 5**  
**Gate verdict: INSUFFICIENT_COMPUTABLE_N**

Human actions that could still change the verdict:
1. Resolve H. glaber method decision (accept with FLAG_TNZ_UNCLEAR) → N=3 → gate passes exactly at threshold → human approval required for Phase 3 at N=3
2. If H. glaber is rejected → gate fails permanently at N=2 with current species set
