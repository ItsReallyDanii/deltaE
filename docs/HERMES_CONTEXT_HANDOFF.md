## Hermes Context Handoff — EME Phase 2A Real ΔE Grounding

## Purpose

This document is a context handoff for Hermes.

It explains the current state of the EME / deltaE project after Phase 2A source verification, audit, Alligator source recovery, and the latest branch rebase/patch resolution.

This file is not an execution instruction file.

It does not authorize Phase 3.

It does not update scientific claims.

It does not request code changes.

It exists so Hermes can understand what has happened so far, what the current project state means, and why the next decision point matters.

## Project Identity

Repository:

```bash
ItsReallyDanii/deltaE
```

Active branch:

```bash
claude/peaceful-gauss-a3xifh
```

Project:

```sql
EME Phase 2 — Real ΔE Grounding
```

Active framing:

```
Epistatic-Metabolic Entanglement / bypass-cost
```

Superseded framing:

```css
Thermodynamic floor / ancestral transition energy
```

The old thermodynamic-floor framing is historical context only. The active metric is not true ancestral-vs-current transition energy.

The current metric is:

```scss
Current Clade-Relative Metabolic Deviation
```

The core computed fields are:

```ini
signed_log_residual = log10(measured_metabolic_rate / predicted_clade_baseline)
delta_E_magnitude = abs(signed_log_residual)
```

This metric measures how far a modern species sits from a clade-relative metabolic baseline. It does not directly measure historical evolutionary transition energy.

## High-Level Scientific State

The project originally had a Phase 3 simulation that produced a sigmoid/threshold-style result from placeholder ΔE values.

That result is no longer treated as a finding.

The placeholder Phase 3 values were:

```csharp
[0.1, 0.25, 0.5, 0.75, 1.0]
```

The old Phase 3 result included a reported threshold near:

```
δE ≈ 0.554
```

and a lock-in ratio around:

```
253,988x
```

Those numbers are retained only as artifact/provenance. They are not evidence for the hypothesis.

The current project state is focused on Phase 2A: determining whether real, source-backed metabolic data exists for the five target species strongly enough to compute real candidate ΔE values.

## Target Species

The five target species are:

# Hermes Context Handoff — EME Phase 2A Real ΔE Grounding

| Species | Current status |
|----------------------------|----------------------------------------------------------------|
| _Crocodylus porosus_ | Compute-eligible, provisional/equation-derived |
| _Alligator mississippiensis_ | Compute-eligible after Gienger et al. 2017 recovery |
| _Heterocephalus glaber_ | Not currently compute-eligible; human method decision required |
| _Dermochelys coriacea_ | Not compute-eligible; structurally absent whole-animal BMR/SMR |
| _Thunnus thynnus_ | Not compute-eligible; proxy-only / non-standard-state data |

The two reference/outgroup species are:

| Species | Current status |
|---------------|---------------------------------------------|
| _Gallus gallus_ | Reference only; not counted toward target N |
| _Mus musculus_ | Reference only; not counted toward target N |

## Phase 2A Deliverables Produced

Phase 2A produced four core data artifacts:

```bash
data/source_verification_matrix.csv
data/allometry_config.json
data/real_delta_e_candidates.csv
data/missing_data_log.md
```

These files were audited and patched.

The initial audit found four issues:

1.  _Mus musculus_ mass field typo: `26500` corrected to `26.5`.
    
2.  _Alligator mississippiensis_ note arithmetic corrected.
    
3.  _Crocodylus porosus_ received missing `FLAG_MASS_NOT_PAIRED_TO_RATE`.
    
4.  _Gallus gallus_ received missing `FLAG_TNZ_UNCLEAR`.
    

After those patches, the Phase 2A audit status became:

```
PASS
```

The gate verdict remained:

```
INSUFFICIENT_COMPUTABLE_N
```

## Alligator Source Recovery

A narrow source-recovery investigation was run for _Alligator mississippiensis_.

The recovered source was:

```scss
Gienger CM, Brien ML, Tracy CR, Manolis SC, Webb GJW, Seymour RS, Christian KA (2017).
Ontogenetic comparisons of standard metabolism in three species of crocodilians.
PLOS ONE 12(2): e0171082.
doi:10.1371/journal.pone.0171082.
PMC5300253.
```

This source provides exact-species _Alligator mississippiensis_ SMR data at 30°C.

The key equation used was:

```ini
SMR = 0.491 × M_kg^0.965 ml O2/min
```

At representative mass:

```ini
M = 100 kg
```

the measured Alligator SMR was calculated as:

```bash
0.491 × 100^0.965 = 41.80 ml O2/min
41.80 × 60 = 2508.1 ml O2/h
```

The clade baseline used was:

```
REP_BD76
```

At:

```css
100,000 g
```

the baseline was:

```bash
0.188 × 100000^0.80 = 1880.0 ml O2/h
```

The resulting residual was:

```ini
signed_log_residual = log10(2508.1 / 1880.0) = 0.1254
delta_E_magnitude = 0.1254
```

The Alligator row is now compute-eligible.

The quality flags include:

```
FLAG_COMPUTE_ELIGIBLE
FLAG_MASS_NOT_PAIRED_TO_RATE
```

The mass/rate caveat remains important: this is an equation-derived representative-mass value, not a single animal paired mass-and-rate measurement.

## Current Gate State

After Alligator source recovery:

```yaml
Target compute-eligible count: 2 / 5
Gate threshold: 3 / 5
Gate verdict: INSUFFICIENT_COMPUTABLE_N
Phase 3 authorized: NO
```

The two compute-eligible target species are:

| Species | δE magnitude | Caveat |
|----------------------------|--------------|--------------------------------------------|
| _Crocodylus porosus_ | 0.1664 | Equation-derived representative-mass value |
| _Alligator mississippiensis_ | 0.1254 | Equation-derived representative-mass value |

The gate still does not pass.

Phase 3 remains blocked.

## Remaining Human Decision Point

The only remaining plausible path to reaching N=3 in the current five-species set is _Heterocephalus glaber_.

The species has existing metabolic data, but it is not currently compute-eligible because of its thermoconforming physiology.

The methodological issue is that _H. glaber_ is a mammal, but it does not behave like a typical strict endotherm. Its metabolic rate tracks ambient temperature more strongly than standard mammalian BMR assumptions expect.

The decision is whether to treat _H. glaber_ as:

```csharp
Accepted compute-eligible with strong thermoconformer flags
```

or:

```sql
Rejected / permanently non-computable under the current Phase 2A criteria
```

Accepting _H. glaber_ would likely bring the target count to:

```
3 / 5
```

but that would be a flagged, minimum-threshold dataset. It would not automatically authorize Phase 3.

## Structurally Non-Computable Species

Two target species remain structurally non-computable under current rules:

### Dermochelys coriacea

The leatherback turtle lacks whole-animal BMR/SMR suitable for this Phase 2A metric.

Available values are field/dive/routine-style metabolic estimates, not accepted baseline BMR/SMR values.

It remains non-computable.

### Thunnus thynnus

Atlantic bluefin tuna lacks a direct exact-species standardized metabolic rate suitable for this calculation.

Available values rely on proxy/sister species, active swimming, routine metabolic states, or temperatures that do not fit the current baseline rules.

It remains non-computable.

## Branch / Git State

The latest relevant patch went through a rebase conflict because both the remote branch and local patch had added:

```
docs/ALLIGATOR_SOURCE_RECOVERY.md
```

The conflict was resolved by preserving the authoritative source-recovery investigation and updating it to reflect that the Alligator patch has now been applied.

The branch now contains the Alligator recovery patch on top of the previous remote state.

The relevant updated documentation files are:

```
docs/ALLIGATOR_SOURCE_RECOVERY.md
docs/PHASE2A_GATE_RESULT.md
```

The Alligator source-recovery file now records:

-   source found
    
-   patch applied
    
-   files changed
    
-   updated target N = 2/5
    
-   Phase 3 still blocked
    

The Phase 2A gate-result file now records:

-   Alligator compute-eligible
    
-   target N updated from 1/5 to 2/5
    
-   gate still insufficient
    
-   _H. glaber_ remains the next human method decision
    

## Current Interpretation

This project has produced a useful scientific/data-quality result:

```kotlin
The original five-species Phase 2A design is not yet data-sufficient for a real-ΔE Phase 3 test.
```

That is not a failure of the control package.

It shows the control system is working: the project did not force a result from weak or proxy data.

The current clean state is:

```csharp
Phase 2A artifacts exist.
Audit status is PASS.
Alligator recovery succeeded.
Target N is now 2/5.
Gate still fails.
Phase 3 remains blocked.
Heterocephalus glaber requires a human method decision.
```

## Important Non-Claims

The current project does not claim:

-   that EME is confirmed
    
-   that bypass complexity scales sigmoidally with real ΔE
    
-   that a real threshold exists
    
-   that the old δE≈0.554 result is biologically meaningful
    
-   that leatherback or tuna can be used via proxy
    
-   that reference species count toward target N
    
-   that Phase 3 is authorized
    
-   that the current metric is ancestral-vs-current transition energy
    

The safest current public wording is:

```csharp
Phase 2A source verification found that the original five-species target set is only partially computable under strict evidence criteria. After Alligator source recovery, 2 of 5 target species are compute-eligible. Phase 3 remains blocked pending a human method decision on Heterocephalus glaber or an explicitly scoped Phase 2B expansion.
```

## Human Owner Context

Project owner:

```
Daniel Sleiman
```

The owner has been using multiple model systems to audit the project, including ChatGPT, Claude, Perplexity/Sonnet, Gemini, Claude Code, and Hermes.

The owner’s preference for this project is:

-   audit-first
    
-   scope-controlled
    
-   no overclaiming
    
-   no forced positive result
    
-   no hidden proxy substitution
    
-   no Phase 3 run until gates are clearly satisfied
    
-   preserve null / blocked outcomes as valid outcomes
    

## Present Standing

The present standing is:

```yaml
Stable checkpoint.
No immediate Phase 3.
Next unresolved scientific/method decision: Heterocephalus glaber.
```