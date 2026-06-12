# 09 — Claim Registry Patch Guide v2

## Purpose

Provide exact guidance for patching stale claims in `README.md` and `docs/CONTROL_PACK.md` without overclaiming.

Use this only after reading `01_REPO_AUDIT.md` and `11_GEMINI_AUDIT_INTEGRATION.md`.

---

## Updated claim terminology

Use:

```text
Current Clade-Relative Metabolic Deviation
```

Avoid:

```text
historical transition ΔE
ancestral-vs-current ΔE
true transition energy
thermodynamic floor proof
```

unless a future ancestral-state reconstruction is actually implemented.

---

## Existing problematic README text

Current README reports:

```text
Sigmoid threshold confirmed at dE=0.554 (253,988x vs linear)
Lock-in speed r=-0.994 with dE magnitude
4 species: zero clean deletions, all bypass constructions confirmed
```

## Recommended replacement README section

```md
## Current Status

This repository contains an early EME prototype.

- Phase 1 qualitative bypass audit: useful but sample-selected.
- Phase 2 real-data grounding: pending.
- Phase 3 CNE simulation: run once with placeholder ΔE values only.

The previous Phase 3 output (`dE≈0.554`, `253,988x vs linear`) is preserved as a placeholder simulation artifact and must not be interpreted as a biological finding.

The next required step is to build a Phase 2 candidate dataset using a defensible current clade-relative metabolic deviation proxy, validate source quality, and only then decide whether a real-input Phase 3 rerun is justified.
```

---

## CONTROL_PACK claim registry patch

### Replace or annotate this row

Current:

```md
| Sigmoid threshold dE=0.554 | SIMULATED | Phase 3 |
```

Recommended:

```md
| Placeholder sigmoid threshold dE≈0.554 / ratio≈253,988x | ARTIFACT_NOTE / SIMULATED_PLACEHOLDER | Phase 3 placeholder ΔE run; not biologically grounded; sensitivity required |
```

### Replace or annotate this row

Current:

```md
| Bypass complexity scales with dE | QUALITATIVE | Phase 1+2 |
```

Recommended:

```md
| Bypass complexity scales with current clade-relative metabolic deviation | HYPOTHESIS_UNVERIFIED | Phase 2 data grounding not yet run; metric is not ancestral transition energy |
```

### Replace or annotate this row

Current:

```md
| No prior quantitative bypass model | ASSESSED NOVEL | May 2026 search |
```

Recommended:

```md
| EME may be complementary to Dubiner/Cubo-style work | ASSUMPTION_REQUIRES_DIFFERENTIATION | Needs explicit novelty comparison and real-data outputs before public claim |
```

---

## Claim tags to use

```text
CONFIRMED
QUALITATIVE
HYPOTHESIS_UNVERIFIED
SIMULATED_PLACEHOLDER
SIMULATED_REAL_INPUT_PRELIMINARY
ARTIFACT_NOTE
ASSESSED_NOVEL_PENDING_REVIEW
BLOCKED_DATA_GAP
INSUFFICIENT_COMPUTABLE_N
NULL_RESULT
```

---

## Rules for future claim updates

If Phase 2A is blocked:

```md
| Real metabolic-deviation grounding incomplete | BLOCKED_DATA_GAP | Missing/partial species metabolic data or unverified allometry |
```

If Phase 2A passes but Phase 3 is not yet run:

```md
| Current clade-relative metabolic deviation dataset created | QUALITATIVE / DATASET_PRELIMINARY | Source-verified candidate data; simulation not yet rerun |
```

If real input produces no threshold:

```md
| Real-input simulation did not support a sigmoid threshold | NULL_RESULT | Real input + sensitivity check |
```

If real input produces an unstable sigmoid:

```md
| Real-input sigmoid result unstable under threshold/epsilon sensitivity | ARTIFACT_PRONE | Sensitivity grid |
```

If real input produces a stable pattern:

```md
| Real-input simulation shows preliminary sigmoid-like scaling | SIMULATED_REAL_INPUT_PRELIMINARY | Real input + sensitivity; still simulation-limited and small-N |
```

---

## Forbidden wording

Do not use:

```text
confirmed threshold
proved irreversibility
validated EME
biological threshold at dE=0.554
253,988x evidence
zero clean deletions generally
novel framework confirmed
thermodynamic floor proven
```

---

## Git diff expectation

A good patch should mostly touch:

```text
README.md
docs/CONTROL_PACK.md
```

It should not rewrite the whole repo.
