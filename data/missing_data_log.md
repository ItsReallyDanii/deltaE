# Missing Data Log — Phase 2A

**Project:** EME Phase 2 — Real ΔE (Delta E) Grounding  
**Metric:** Current Clade-Relative Metabolic Deviation  
**Formula:** `signed_log_residual = log10(metabolic_rate_standardized / predicted_metabolic_rate)`  
**Generated:** Phase 2A source verification pass  
**Status:** 1 of 5 target species compute-eligible; gate INSUFFICIENT_COMPUTABLE_N

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

- **Field missing:** Standardized metabolic rate measurement at the reference temperature (30°C) required by the clade-level reptile allometric baseline (REP_BD76, Bennett & Dawson 1976)
- **Status:** DATA EXISTS AT WRONG TEMPERATURE — AnAge reports resting MR at 22.2°C; reference temperature is 30°C; no verified Q10 correction equation in allometry_config.json
- **Attempted sources:**
  - AnAge database (https://genomics.senescence.info/species/entry.php?species=Alligator_mississippiensis): resting MR = 0.1539 W at 1079 g, temperature 22.2°C — temperature mismatch
  - Coulson & Hernandez (1983) Comp Biochem Physiol 74A:1-182: extensive alligator metabolic study; full-text access needed to confirm whether 30°C measurements are reported
  - Bennett & Dawson (1976) Biology of the Reptilia: provides clade equation at 30°C; does not provide A. mississippiensis specific measurements
- **Available alternative data:**
  - AnAge value: 0.1539 W at 22.2°C (resting MR, 1079 g) — documented but not computable
  - For reference (not used): REP_BD76 at 1079 g predicts 0.188 * 1079^0.80 = 0.188 * 214.4 = 40.3 ml O₂/h at 30°C; measured is 27.56 ml O₂/h at 22.2°C — gap primarily temperature-driven
- **Reason for exclusion:** The reptile SMR allometric baseline (REP_BD76) is calibrated at 30°C. Applying it to a measurement taken at 22.2°C would produce a delta_E that conflates a 7.8°C temperature effect with species-level metabolic deviation. Ectotherm metabolic rates decrease substantially with temperature (Q₁₀ ≈ 2–3 for reptiles at 20–30°C); applying no correction would artificially suppress the measured value and generate a misleading negative residual. No Q10 or Arrhenius correction equation is currently authorized in allometry_config.json.
- **Fallback used:** AnAge data documented in real_delta_e_candidates.csv with `compute_eligible=FALSE` and `FLAG_TEMP_UNCORRECTED_REVIEW_REQUIRED`.
- **Human review needed:** Yes — two paths exist: (a) locate A. mississippiensis SMR measurement at 30°C in Coulson & Hernandez (1983) or other primary literature and return for compute-eligibility reassessment; (b) authorize addition of a verified reptile Q10 correction equation to allometry_config.json, citing a primary source (e.g., Coulson 1984, Bennett & Dawson 1976, or Grigg & Seebacher papers), then recompute with FLAG_TEMP_NORMALIZED. Path (a) is strongly preferred per control package rules.
- **Impact on Phase 3 readiness:** If resolved via Path (a) or (b), A. mississippiensis could become the second compute-eligible target species. Together with C. porosus this would give N=2 — still below the gate threshold of 3. Both crocodilians being resolved is required to approach the gate, and even then H. glaber human-method-decision would need resolution to reach N=3.

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
| Alligator mississippiensis | FALSE | Temperature mismatch (22.2°C vs 30°C ref) | Yes — locate 30°C primary lit or authorize Q10 correction |
| Heterocephalus glaber | FALSE | Thermoconforming physiology — human method decision | Yes — pending human decision |
| Dermochelys coriacea | FALSE | No whole-animal BMR exists; technically infeasible | No — structurally absent |
| Thunnus thynnus | FALSE | No direct species rate; proxy + non-standard state + wrong temp + no baseline equation | No — structurally absent |
| Gallus gallus (reference) | TRUE | — | N/A |
| Mus musculus (reference) | TRUE | — | N/A |

**Target species compute-eligible: 1 of 5**  
**Gate threshold: 3 of 5**  
**Gate verdict: INSUFFICIENT_COMPUTABLE_N**

Human actions that could change the verdict:
1. Locate A. mississippiensis measurement at 30°C → N=2
2. Resolve H. glaber method decision (accept) → N=2 (if AM not resolved) or N=3 (if AM also resolved)
3. Both actions → N=3 → gate passes exactly at threshold → human approval required for Phase 3 at N=3
