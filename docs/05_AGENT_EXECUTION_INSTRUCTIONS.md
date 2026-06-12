# 05 — Agent Execution Instructions v2

## Purpose

Give coding agents, Hermes, Claude Code, or another LLM bounded execution instructions for EME Phase 2.

The job is not to rescue the hypothesis. The job is to build an auditable data-grounding layer.

---

## Execution metadata

| Field | Recommendation |
|---|---|
| Estimated task time | 2–5 hours for Phase 2A; longer only if source access is messy |
| Model | Normal coding model / Sonnet normal-high; Fable not needed initially |
| Effort | Medium; high only for source/equation disputes |
| Thinking/reasoning | Use for internal checking; return concise audit trail |
| Token risk | Low if files are read selectively |
| Tools required | File read/write, Python, shell, pandas/numpy, JSON/CSV validation |
| Cost caveat | Do not run open-ended autonomous sessions. |

---

## Operating role

You are executing a bounded Phase 2 control package for EME real-data grounding.

Your job is to:

1. Preserve scope.
2. Challenge weak claims.
3. Implement source verification and data schema first.
4. Avoid silent biological proxy substitution.
5. Stop when uncertainty is scientifically consequential.

---

## Must read first

Read these files in order:

```text
00_SCOPE_CLAIMS_AND_HANDOFF_BRIEF.md
11_GEMINI_AUDIT_INTEGRATION.md
13_PHASE2_DATA_SCHEMA.md
03_IMPLEMENTATION_PLAN.md
04_VALIDATION_PLAN.md
```

If files conflict, pause and report the conflict.

---

## Allowed actions

You may:

- inspect files,
- add data schema validation,
- create `data/source_verification_matrix.csv`,
- create `data/allometry_config.json`,
- create `data/real_delta_e_candidates.csv`,
- create `data/missing_data_log.md`,
- add small helper functions/scripts,
- run local validation scripts,
- update README/CONTROL_PACK only after validation.

---

## Forbidden actions

Do not:

- edit or generate a Hermes `SOUL.md`,
- revive the old closed-system thermodynamic-floor framing,
- call the metric true ancestral transition ΔE,
- hard-code allometric constants in Python,
- use AmphiBIO/EltonTraits/TimeTree as metabolic-rate sources,
- use TimeTree for anything other than divergence metadata,
- silently substitute sister/proxy species,
- compute ΔE from FMR/MMR/DMR/active/postprandial/SDA rows by default,
- run Phase 3 before the Phase 2A gate passes,
- tune simulation constants to force a sigmoid,
- call the placeholder Phase 3 result confirmed,
- draft a paper/public claim before validation,
- push to GitHub without explicit human approval.

---

## Required output after execution

Return:

1. What I inspected
2. What I changed
3. Commands run
4. Outputs created
5. Evidence / file paths
6. Species/data rows that are compute-eligible
7. Species/data rows excluded and why
8. Risks or unresolved questions
9. Whether Phase 3 is allowed, blocked, or human-review-required
10. Recommended next action

---

## Stop-and-ask triggers

Stop if:

- selected source lacks exact species match,
- metabolic state is unclear,
- measurement temperature is missing for an ectotherm,
- allometry constants are unverified,
- mass and metabolic rate are from different studies without justification,
- fewer than 3 target species are compute-eligible,
- any instruction, script, or judgment call would loosen/reinterpret compute-eligibility criteria to reach N≥3,
- any file says the placeholder Phase 3 result is confirmed,
- the output wording starts sounding publishable before validation.

---

## Minimal good behavior

A good result:

> “I created the source matrix and candidate dataset. Only 3/5 target species are compute-eligible. Leatherback and tuna are logged as non-computable/proxy-risk. Phase 3 requires human review before running.”

An equally valid result:

> “Only 1–2/5 target species are compute-eligible under the current criteria. I did not loosen criteria to force N≥3. I produced `INSUFFICIENT_COMPUTABLE_N` and a data-gap report instead of running Phase 3.”

A bad result:

> “I filled missing values from sister species and recovered the sigmoid threshold.”

---

## Hermes-specific note

If running through Hermes Telegram, ask for approval before writes or any Phase 3 rerun. If running through SSH/Termius, inspect and run commands directly but summarize before mutating files. Cron should be read-only until Phase 2A implementation is stable.
