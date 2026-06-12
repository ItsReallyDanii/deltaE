# 10 — Handoff Prompt for Next LLM v2

## Purpose

Copy this into another LLM when you want it to audit or execute the package.

For Claude-specific audit, prefer `12_CLAUDE_AUDIT_PROMPT.md`.

---

## Execution metadata

| Field | Recommendation |
|---|---|
| Suggested model | Claude Sonnet 4.6 high/normal-high, ChatGPT strong reasoning, or equivalent |
| Effort | High for audit; medium for implementation |
| Thinking/reasoning | Yes, but request concise audit output |
| Fable/Max needed? | No unless this package conflicts with later results |
| Token risk | Medium if all files pasted; lower if ZIP/key files only |
| Cost caveat | Paste only task-relevant files, not the entire chat history. |

---

## Copy-paste prompt

You are receiving an EME Phase 2 real-data grounding control package v2.

Your job is not to agree with it blindly. Your job is to preserve useful context, challenge weak claims, identify missing evidence, and propose or execute the minimal bounded next step.

### Project summary

The project is **EME Phase 2 — Real ΔE Grounding**, but the current implementation metric should be called **Current Clade-Relative Metabolic Deviation**, not true historical transition ΔE.

The goal is to replace placeholder ΔE values in an existing Phase 3 CNE dependency-accumulation simulation with source-verified, biologically defensible metabolic-deviation values for the already named species, then decide whether a real-input simulation is justified.

The original v0.1 framing used “thermodynamic floor” language inspired by ritonavir polymorphism, but that framing has been superseded. Do not revive it.

### Master constraints

- Do not edit or generate a Hermes `SOUL.md`.
- Do not expand the species list silently.
- Do not treat the placeholder Phase 3 result as confirmed.
- Do not hard-code allometry constants in Python.
- Do not compute from proxy/sister-species values by default.
- Do not compute from FMR/MMR/DMR/active/postprandial data by default.
- Preserve `signed_log_residual` and `delta_E_magnitude` separately.
- “No threshold found” or “data insufficient” is a valid outcome.
- If source data is missing, document the gap explicitly.

### Files to read first

Read in this order:

1. `00_SCOPE_CLAIMS_AND_HANDOFF_BRIEF.md`
2. `11_GEMINI_AUDIT_INTEGRATION.md`
3. `13_PHASE2_DATA_SCHEMA.md`
4. `03_IMPLEMENTATION_PLAN.md`
5. `04_VALIDATION_PLAN.md`
6. `05_AGENT_EXECUTION_INSTRUCTIONS.md`

### Your first response must include

1. Understanding of the project
2. Strongest confirmed claims
3. Weak or unverified claims
4. Risks
5. Recommended next step
6. What you would not build yet
7. Questions that actually matter
8. Whether research, implementation, or validation is next

### If executing

Before editing files, state:

- files you intend to modify,
- commands you intend to run,
- outputs you intend to create,
- stop conditions.

After execution, return:

1. What you inspected
2. What you changed
3. Commands run
4. Outputs created
5. Evidence/file paths
6. Rows compute-eligible vs excluded
7. Risks or unresolved questions
8. Whether human review is required
9. Recommended next action

### Important

If the current result is null, unstable, blocked, or small-N, report that plainly. Do not rescue the hypothesis.
