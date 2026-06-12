# 01 — Repository Audit: `eme_project_v03.zip`

## Purpose

Audit the uploaded prototype ZIP so the next agent understands what already exists, what is stale, and what must be repaired before implementation.

This file is descriptive. It should not be treated as implementation permission.

---

## Execution metadata

| Field | Recommendation |
|---|---|
| Estimated human read time | 8–12 minutes |
| Estimated agent work time | 10–20 minutes |
| Model needed | Normal reasoning model; no Claude Max/Fable |
| Reasoning level | Medium |
| Token risk | Low; current ZIP is tiny |
| Tools required | unzip, Python text inspection |
| Cost caveat | Do not upload full unrelated chat history to expensive model for this audit. |

---

## Current ZIP structure

```text
eme_project/README.md
eme_project/docs/CONTROL_PACK.md
eme_project/data/species_bypass_table.csv
eme_project/simulation/phase2_data_pull.py
eme_project/simulation/cne_simulation.py
eme_project/outputs/phase3_results.json
eme_project/outputs/phase3_cne_simulation.png
```

## Key file findings

### `README.md`

The README currently reports the following as key results:

```text
Sigmoid threshold confirmed at dE=0.554 (253,988x vs linear)
Lock-in speed r=-0.994 with dE magnitude
4 species: zero clean deletions, all bypass constructions confirmed
```

**Audit finding:** this is stale and overconfident. The current master scope says Phase 3 used placeholder ΔE values and must be treated as a methodological artifact until real ΔE values are used.

### `docs/CONTROL_PACK.md`

The current claim registry includes:

```text
Sigmoid threshold dE=0.554 | SIMULATED | Phase 3
Bypass complexity scales with dE | QUALITATIVE | Phase 1+2
No prior quantitative bypass model | ASSESSED NOVEL | May 2026 search
```

**Audit finding:** `SIMULATED` is acceptable only if the label clearly says placeholder/synthetic. The current text does not sufficiently warn that the result is artifact-prone and not biologically grounded.

### `simulation/phase2_data_pull.py`

Current state:

- Pulls/matches EltonTraits body-mass data if `MamFuncDat.txt` exists locally.
- Queries TimeTree for only two species pairs.
- Prints AmphiBIO download instructions.
- Does **not** compute real ΔE.
- Does **not** produce a final `real_delta_e.csv`.
- Does **not** validate species coverage across AmphiBIO, EltonTraits, and TimeTree.

**Audit finding:** this is a skeleton. It is not Phase 2 completion.

### `simulation/cne_simulation.py`

Current state:

```python
DELTA_E_LEVELS = [0.1, 0.25, 0.5, 0.75, 1.0]
```

Other important constants:

```text
mutation rate default: 0.003
lock-in threshold default: 0.50
dependency scaling internal constant: 0.015
exponent in attachment probability: 1.5
epsilon in ratio denominator: 1e-9
```

**Audit finding:** architecture is usable, but the ΔE axis is placeholder and the ratio calculation is sensitive to near-zero sigmoid residuals.

### `outputs/phase3_results.json`

Current results:

```json
{
  "delta_e_levels": [
    0.1,
    0.25,
    0.5,
    0.75,
    1.0
  ],
  "pct_locked": [
    0.0,
    0.0,
    21.5,
    99.06666666666666,
    100.0
  ],
  "avg_lock_gen": [
    null,
    null,
    6793.981395348837,
    3909.0921938088827,
    1927.7753333333333
  ],
  "sigmoid_params": {
    "L": 100.0,
    "k": 23.87,
    "x0": 0.554,
    "threshold_de": 0.554
  },
  "linearity_ratio": 253988.4,
  "verdict": "SIGMOIDAL THRESHOLD CONFIRMED"
}
```

**Audit finding:** output confirms the stale headline. Preserve for provenance, but do not cite as biology.

---

## Species table in prototype

```csv
species,estimated_delta_e,bypass_structures_count,hardware_retained,bypass_type,source
Crocodylus porosus,0.75,3,four-chambered heart,cardiovascular shunt,"Seymour et al 2004"
Alligator mississippiensis,0.75,3,four-chambered heart,cardiovascular shunt,"Seymour et al 2004"
Heterocephalus glaber,0.25,1,functional UCP1 BAT,behavioral outsourcing,"PNAS 2026"
Dermochelys coriacea,0.50,2,countercurrent exchanger,gigantothermy+CCHE,"Royal Soc 2015"
Thunnus thynnus,0.50,1,retia mirabilia,regional vascular isolation,"BMC Genomics 2020"
```

Audit notes:

- These rows define the bounded species list for Phase 2.
- Do not add species unless a data gap forces a clearly documented fallback proposal.
- `estimated_delta_e` is placeholder/proxy only, not a real BMR-derived value.

---

## Required repairs before external-facing claims

1. README must label current Phase 3 as **placeholder/synthetic**.
2. CONTROL_PACK claim registry must demote or annotate the sigmoid result.
3. `phase2_data_pull.py` must produce explicit real ΔE values or explicit data-gap rows.
4. `cne_simulation.py` must accept real ΔE input from a data file.
5. New outputs must include sensitivity checks.
6. Any null/no-threshold result must be preserved.

---

## Verdict

The prototype is useful but claim-stale.

**Proceed only as a Phase 2 grounding task.**

Do not use this ZIP as a paper-ready, demo-ready, or claim-ready artifact.
