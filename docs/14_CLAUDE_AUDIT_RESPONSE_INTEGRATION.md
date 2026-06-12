# 14 — Claude Audit Response Integration v2.1

## Purpose

This file records the post-v2 Claude audit result and the minimal patches applied before Phase 2A handoff.

Claude's audit verdict was: **minor patch, otherwise safe to hand to a coding agent for Phase 2A only**.

## Accepted audit findings

The v2 package was judged internally consistent and safe for Phase 2A source/data verification, with Phase 3 correctly gated. The strongest points identified were:

- the reframe from historical/ancestral ΔE to **Current Clade-Relative Metabolic Deviation**,
- the strict demotion of the placeholder Phase 3 sigmoid/ratio result to artifact/provenance status,
- machine-checkable Phase 2A gates,
- explicit prohibitions against hard-coded allometry, silent proxy substitution, and mixed physiological states,
- claim-registry guardrails against overclaiming.

## Patches applied in v2.1

### 1. Phase 2A null/data-gap outcome made explicit

`00_SCOPE_CLAIMS_AND_HANDOFF_BRIEF.md` now states that a Phase 2A outcome with only 0–2 compute-eligible species is valid and expected if exact-species data, ectotherm temperature handling, or source metadata are insufficient.

### 2. Anti-gaming rule added

`03_IMPLEMENTATION_PLAN.md` and `05_AGENT_EXECUTION_INSTRUCTIONS.md` now explicitly state:

> Compute-eligibility criteria must not be loosened, reinterpreted, or special-cased to reach the N≥3 threshold; report `INSUFFICIENT_COMPUTABLE_N` honestly if the gate fails on its own terms.

### 3. Conflicting-source tie-break rule added

`03_IMPLEMENTATION_PLAN.md` now includes a source-conflict rule: prefer peer-reviewed primary literature, exact-species measurements, same-study mass/rate pairs, correct physiological state, and better methodology. All conflicts must be logged in `source_verification_matrix.csv` and `missing_data_log.md`.

## Phase 2A status after patch

After these patches, the package is ready for a bounded coding-agent/Hermes Phase 2A task:

- build source verification matrix,
- populate candidate metabolic rows,
- apply schema and quality flags,
- produce data-gap report if N<3,
- do not run Phase 3 without human approval.

## Explicitly not authorized yet

- Phase 3 rerun,
- claim registry rewrite,
- README scientific-claim update,
- generic Q10/Arrhenius temperature-correction framework,
- novelty/public-positioning writeup.
