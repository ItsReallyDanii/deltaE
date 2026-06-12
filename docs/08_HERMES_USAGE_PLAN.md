# 08 — Hermes Usage Plan

## Purpose

Define how Hermes should help with EME Phase 2 through Telegram, SSH/Termius, and cron without turning into uncontrolled repo automation.

---

## Execution metadata

| Field | Recommendation |
|---|---|
| Estimated setup time | 15–30 minutes |
| Model | Hermes default / cheap coding model for routine checks |
| Effort | Low for cron; medium for repo inspection; high only for final audit summary |
| Token risk | Low if cron is script-first |
| Tools required | Hermes gateway, Telegram, SSH/Termius, local repo clone |
| Cost caveat | Keep cron script-first; invoke LLM only on changed/failed states. |

---

## Principle

Hermes should act as a bounded operator:

```text
Scripts detect.
Hermes summarizes.
Daniel approves.
Agents modify only after approval.
```

---

## Do not edit soul

This package does not require a `SOUL.md` change.

Do not ask Hermes to rewrite its personality or operating identity for this project.

---

## Best Hermes use cases for this package

### Telegram

Use for:

- quick status queries,
- approval gates,
- receiving audit summaries,
- deciding whether to run the next task.

Example:

```text
Read 01_REPO_AUDIT.md and tell me whether the repo is safe to implement Phase 2 yet. Do not edit files.
```

### SSH / Termius

Use for:

- checking files,
- running Python scripts,
- inspecting generated CSV/JSON,
- debugging local paths.

Example:

```bash
python simulation/phase2_data_pull.py
python simulation/cne_simulation.py --help
```

### Cron

Use later, not now, for:

- weekly repo consistency check,
- stale claim check,
- missing deliverables reminder,
- output-file existence check.

Do not schedule autonomous scientific interpretation as cron.

---

## Suggested Hermes tasks

### Task 1 — Read-only package audit

```text
Read the EME Phase 2 control package. Report conflicts between README.md, CONTROL_PACK.md, and 00_SCOPE_CLAIMS_AND_HANDOFF_BRIEF.md. Do not edit files.
```

### Task 2 — Data readiness check

```text
Inspect whether required local data files for AmphiBIO, EltonTraits, and TimeTree are present. Report missing files and expected paths. Do not download or infer data.
```

### Task 3 — Implementation after approval

```text
Using 03_IMPLEMENTATION_PLAN.md, modify only the minimum files needed to produce data/real_delta_e.csv and allow cne_simulation.py to accept real ΔE input. Stop before updating claims.
```

### Task 4 — Validation

```text
Run the real-ΔE simulation and sensitivity checks described in 04_VALIDATION_PLAN.md. Produce JSON outputs and summarize whether the result is stable, unstable, null, or blocked.
```

### Task 5 — Claim patch

```text
Using 09_CLAIM_REGISTRY_PATCH_GUIDE.md, update README.md and docs/CONTROL_PACK.md conservatively based on validated outputs. Do not add external-facing publication claims.
```

---

## Cron ideas after implementation exists

### Weekly stale-claim audit

```text
Check whether README.md or docs/CONTROL_PACK.md contains any of: CONFIRMED sigmoid, 253,988x, dE=0.554, or threshold confirmed without artifact caveat. If found, notify Daniel and do not edit automatically.
```

### Deliverables check

```text
Check whether data/real_delta_e.csv, outputs/phase3_real_delta_e_results.json, and outputs/phase3_real_delta_e_sensitivity.json exist. Notify only if missing.
```

### Test/run check

```text
Run a smoke test for cne_simulation.py in placeholder mode and real-ΔE mode if data exists. Notify on failure.
```

---

## Approval gates

Require explicit human approval before:

- downloading large datasets,
- changing repo files,
- updating claim labels,
- deleting outputs,
- committing/pushing,
- expanding species list,
- using expensive models.

---

## Expected Hermes summary format

Hermes should reply with:

```text
Status: PASS / BLOCKED / NEEDS REVIEW
Files inspected:
Commands run:
Findings:
Risks:
Next recommended action:
Approval needed: yes/no
```
