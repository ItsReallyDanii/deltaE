# Heterocephalus glaber — Human Method Decision

## Purpose

This document records the required human method decision for whether *Heterocephalus glaber* may be treated as compute-eligible for Phase 2A Real ΔE Grounding.

This is not a Phase 3 authorization document.

This decision does not update claims, does not validate the EME hypothesis, and does not authorize a Phase 3 rerun by itself.

---

## Current Gate Context

Phase 2A has completed source verification and candidate ΔE computation.

Current status after Alligator source recovery is expected to be:

| Field                                                  | Value                                                 |
| ------------------------------------------------------ | ----------------------------------------------------- |
| Phase                                                  | 2A — Source Verification and Candidate ΔE Computation |
| Target species compute-eligible before Alligator patch | 1 of 5                                                |
| Target species compute-eligible after Alligator patch  | 2 of 5                                                |
| Gate threshold                                         | 3 of 5                                                |
| Phase 3 authorized                                     | NO                                                    |
| Current gate verdict                                   | INSUFFICIENT_COMPUTABLE_N                             |

The only remaining plausible path to N=3 is a human method decision on *Heterocephalus glaber*.

---

## Species Under Review

| Field                                | Value                                                                                  |
| ------------------------------------ | -------------------------------------------------------------------------------------- |
| Scientific name                      | *Heterocephalus glaber*                                                                |
| Common name                          | Naked mole-rat                                                                         |
| Role                                 | Target species                                                                         |
| Current compute status               | Not compute-eligible                                                                   |
| Current blocker                      | Thermoconforming physiology / unclear applicability of standard mammalian TNZ baseline |
| Candidate source                     | O'Connor et al. 1999 / naked mole-rat metabolic physiology literature                  |
| Candidate physiological state        | BMR/RMR-like oxygen consumption                                                        |
| Candidate measurement temperature    | Approximately 30°C                                                                     |
| Candidate approximate metabolic rate | ~13.8 ml O₂/h for ~35 g individual, per existing Phase 2A notes                        |
| Current quality issue                | `FLAG_TNZ_UNCLEAR`                                                                     |

---

## Core Method Question

Should *Heterocephalus glaber* be accepted as compute-eligible despite its unusual thermoconforming mammalian physiology?

The methodological issue is that *H. glaber* is a mammal, but it does not behave like a normal strict endotherm. Its metabolic rate tracks ambient temperature more strongly than typical mammals, and the usual mammalian thermoneutral-zone interpretation is not straightforward.

Using a standard mammalian baseline may produce a large negative residual. That residual could be interpreted in two ways:

1. **Valid EME signal:** an extreme clade-relative metabolic suppression that reflects a real bypass/entanglement state.
2. **Method artifact:** a mismatch between a standard mammalian baseline and an unusual thermoconforming physiology.

---

## Decision Options

### Option A — Accept as Compute-Eligible With Strong Flags

Accept the *H. glaber* value as compute-eligible.

Required interpretation:

> The residual is not treated as ordinary mammalian BMR. It is treated as a flagged, extreme clade-relative metabolic suppression case.

Required flags:

* `FLAG_COMPUTE_ELIGIBLE`
* `FLAG_TNZ_UNCLEAR`
* `FLAG_THERMOCONFORMER`
* `FLAG_METHOD_DECISION_REQUIRED_ACCEPTED`
* `FLAG_INTERPRETATION_CAUTION`

Required note:

> Accepted by explicit human method decision despite thermoconforming physiology. Residual should be interpreted as current clade-relative metabolic suppression, not ancestral transition energy and not ordinary mammalian BMR equivalence.

Gate impact:

| Field                                | Value                                                 |
| ------------------------------------ | ----------------------------------------------------- |
| Target N after Alligator patch       | 2 of 5                                                |
| Target N after accepting *H. glaber* | 3 of 5                                                |
| Gate threshold                       | 3 of 5                                                |
| Phase 3 status                       | Still requires explicit human approval before running |

This option allows the Phase 2A data gate to reach the minimum threshold, but only with a heavily flagged edge-case row.

---

### Option B — Keep Non-Computable

Keep *H. glaber* non-compute-eligible.

Required interpretation:

> Because *H. glaber* thermoconforming physiology makes standard mammalian BMR baseline comparison methodologically ambiguous, it should remain recorded but excluded from ΔE computation.

Required flags:

* `FLAG_NOT_COMPUTE_ELIGIBLE`
* `FLAG_TNZ_UNCLEAR`
* `FLAG_THERMOCONFORMER`
* `FLAG_METHOD_DECISION_REJECTED`

Required note:

> Excluded by human method decision. Data exists, but the standard mammalian baseline comparison is not considered defensible for compute eligibility without a more specific fossorial/thermoconforming mammal baseline.

Gate impact:

| Field                                | Value   |
| ------------------------------------ | ------- |
| Target N after Alligator patch       | 2 of 5  |
| Target N after rejecting *H. glaber* | 2 of 5  |
| Gate threshold                       | 3 of 5  |
| Phase 3 status                       | Blocked |

This option preserves stricter methodological caution but leaves the current five-species set data-insufficient.

---

## Recommended Decision

Recommended decision:

**Option A — Accept as Compute-Eligible With Strong Flags**

Rationale:

*Heterocephalus glaber* is not a random bad data point. It is one of the intentionally selected EME target taxa because it represents extreme metabolic suppression within Mammalia. Excluding it solely because it is biologically unusual may remove exactly the signal the project is trying to test.

However, acceptance must be narrow and heavily caveated:

* Do not treat it as ordinary mammalian BMR.
* Do not treat it as ancestral-vs-current ΔE.
* Do not remove `FLAG_TNZ_UNCLEAR`.
* Do not use it as evidence that the EME hypothesis is confirmed.
* Do not run Phase 3 without explicit human approval after patching.
* Do not hide that N=3 is achieved only by accepting a flagged edge-case row.

Recommended language:

> *H. glaber* is accepted as compute-eligible by explicit human method decision because its thermoconforming metabolic suppression is central to the EME target set. The resulting residual is valid only as a flagged current clade-relative metabolic deviation and must not be interpreted as ordinary mammalian BMR equivalence or historical transition energy.

---

## Required Patch Plan If Option A Is Accepted

Update `data/real_delta_e_candidates.csv`:

* Set *H. glaber* `compute_eligible` to `TRUE`.
* Preserve or populate:

  * `signed_log_residual`
  * `delta_E_magnitude`
  * `predicted_metabolic_rate`
  * `predicted_metabolic_rate_units`
* Add quality flags:

  * `FLAG_COMPUTE_ELIGIBLE`
  * `FLAG_TNZ_UNCLEAR`
  * `FLAG_THERMOCONFORMER`
  * `FLAG_METHOD_DECISION_REQUIRED_ACCEPTED`
  * `FLAG_INTERPRETATION_CAUTION`
* Notes must explicitly state:

  * accepted by human method decision
  * thermoconformer caveat retained
  * current clade-relative metabolic deviation only
  * not ancestral transition energy

Update `data/missing_data_log.md`:

* Change *H. glaber* status from unresolved to accepted-by-human-method-decision.
* Preserve the methodological concern.
* State that acceptance raises target compute-eligible N to 3/5 if the Alligator patch is already applied.
* State that Phase 3 still requires explicit human approval.

Update `docs/PHASE2A_GATE_RESULT.md`:

* Add addendum:

  * Alligator recovery plus *H. glaber* method decision brings target N to 3/5.
  * Gate threshold is reached.
  * Phase 3 is still not automatically authorized.
  * Human approval is required before any Phase 3 execution.

Do not update claim registry yet.

Do not run Phase 3 in this patch.

---

## Required Patch Plan If Option B Is Chosen

Update `data/missing_data_log.md`:

* Record that *H. glaber* was reviewed and rejected for compute eligibility by human method decision.
* State that target N remains 2/5 after Alligator patch.
* State that the current five-species set remains data-insufficient.
* State that Phase 3 remains blocked.

Update `docs/PHASE2A_GATE_RESULT.md`:

* Add addendum:

  * *H. glaber* reviewed and remains non-computable.
  * Gate remains `INSUFFICIENT_COMPUTABLE_N`.
  * Phase 3 remains blocked.
  * Next possible path would require explicit Phase 2B scope expansion.

Do not update claim registry.

Do not run Phase 3.

---

## Decision Record

Human decision:

* [ ] Option A — Accept as compute-eligible with strong flags.
* [ ] Option B — Keep non-computable.

Decision date:

```text
YYYY-MM-DD
```

Decision owner:

```text
Daniel Sleiman
```

Decision rationale:

```text
TBD
```

---

## Phase 3 Authorization

This document does not authorize Phase 3.

Even if Option A is selected and target N reaches 3/5, Phase 3 may run only after a separate explicit approval step.

Required approval language:

```text
I explicitly approve Phase 3 real-ΔE rerun using the current flagged Phase 2A dataset, including the accepted H. glaber thermoconformer caveat.
```

Until that approval exists, Phase 3 remains blocked.
