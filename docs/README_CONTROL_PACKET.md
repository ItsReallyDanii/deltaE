# EME Phase 2 — Real ΔE Grounding Control Package v2

**Package date:** 2026-06-12  
**Version:** v2, post-Perplexity + Gemini audit integration  
**Purpose:** Convert the EME prototype into a bounded, auditable Phase 2 data-grounding package.  
**Master authority:** `00_SCOPE_CLAIMS_AND_HANDOFF_BRIEF.md`  
**Soul handling:** Do **not** edit, rewrite, infer, or create a Hermes `SOUL.md` from this package.

---

## What changed in v2

This version integrates the useful parts of the Perplexity/Sonnet deep-research memo and the Gemini audit while stripping overconfident language.

Major changes:

1. The active metric is no longer described as true historical “transition ΔE.”
2. The implementation term is now: **Current Clade-Relative Metabolic Deviation**.
3. The pipeline must preserve both:
   - `signed_log_residual`
   - `delta_E_magnitude`
4. The first coding task is **Phase 2A source/data verification**, not a Phase 3 simulation rerun.
5. AmphiBIO / EltonTraits / TimeTree are not primary metabolic-rate sources.
6. Required outputs now include:
   - `data/source_verification_matrix.csv`
   - `data/allometry_config.json`
   - `data/real_delta_e_candidates.csv`
   - `data/missing_data_log.md`
7. Proxy/sister-species, FMR, MMR, active, postprandial, and diving-only values must be logged but not used for ΔE computation unless a human explicitly approves an exploratory/proxy run.
8. No allometric constants may be hard-coded in Python.
9. Temperature and physiological-state handling are now validation gates.
10. The old Phase 3 sigmoid/threshold result remains artifact/provenance only.

---

## Package contents

| File | Use |
|---|---|
| `00_SCOPE_CLAIMS_AND_HANDOFF_BRIEF.md` | Master project scope and claim boundary. Later files must obey this. |
| `01_REPO_AUDIT.md` | Audit of uploaded `eme_project_v03.zip` and stale claim conflicts. |
| `02_DEEP_RESEARCH_PROMPT.md` | Original narrow research prompt; retained for provenance. |
| `03_IMPLEMENTATION_PLAN.md` | Updated v2 implementation plan: Phase 2A data verification first. |
| `04_VALIDATION_PLAN.md` | Updated v2 validation gates and quality checks. |
| `05_AGENT_EXECUTION_INSTRUCTIONS.md` | Updated bounded instructions for Hermes / Claude Code / coding agents. |
| `06_DELIVERABLES_CHECKLIST.md` | Required deliverables before any claim update or Phase 3 run. |
| `07_MODEL_COST_GUIDANCE.md` | Updated model strategy and Claude usage guidance. |
| `08_HERMES_USAGE_PLAN.md` | Hermes Telegram/SSH/cron usage boundaries. |
| `09_CLAIM_REGISTRY_PATCH_GUIDE.md` | How to patch README/CONTROL_PACK without overclaiming. |
| `10_HANDOFF_PROMPT_FOR_NEXT_LLM.md` | General handoff prompt for an external LLM. |
| `11_GEMINI_AUDIT_INTEGRATION.md` | What was accepted/rejected from Gemini audit. |
| `12_CLAUDE_AUDIT_PROMPT.md` | Copy-paste prompt for Claude audit of this v2 package. |
| `13_PHASE2_DATA_SCHEMA.md` | Canonical output schema and flags. |
| `templates/allometry_config_template.json` | Template for allometric baseline definitions. |
| `templates/real_delta_e_candidates_schema.csv` | CSV header template for Phase 2 candidate data. |
| `research_memos/` | Perplexity and Gemini memo copies for traceability. |
| `MANIFEST.md` | Inventory and global rules. |

---

## Critical project boundary

The existing Phase 3 output says the placeholder simulation found a sigmoid threshold at ΔE≈0.554 with a 253,988x ratio vs. linear. This must **not** be treated as a biological finding.

It is only:

> a placeholder simulation artifact requiring real data grounding, data-quality gating, and sensitivity testing.

---

## Recommended order of use

1. Read `00_SCOPE_CLAIMS_AND_HANDOFF_BRIEF.md`.
2. Read `11_GEMINI_AUDIT_INTEGRATION.md` for v2 corrections.
3. Read `13_PHASE2_DATA_SCHEMA.md`.
4. Execute `03_IMPLEMENTATION_PLAN.md` through Phase 2A only.
5. Validate with `04_VALIDATION_PLAN.md`.
6. If the data gate passes, then consider a real-ΔE Phase 3 rerun.
7. Update claims with `09_CLAIM_REGISTRY_PATCH_GUIDE.md` only after validation.
8. Send `12_CLAUDE_AUDIT_PROMPT.md` + this package to Claude for an external audit if desired.

---

## Execution metadata

| Field | Recommendation |
|---|---|
| Human review needed? | Yes, before any Phase 3 rerun or claim update. |
| Default model | Normal strong reasoning/coding model; Claude Max/Fable not needed initially. |
| Claude audit model | Sonnet 4.6 high/normal first; Opus 4.8 low only for second-pass critique if credit budget allows. |
| Reasoning effort | Medium-high for audit; medium for coding; high only for source/equation disputes. |
| Token/cost risk | Low if files are read selectively; high if dumping entire history into Max/Fable. |
| Required tools | Python, pandas, numpy, requests, local CSV/JSON; optional dataset/manual literature downloads. |
| Forbidden actions | Do not edit soul, revive thermodynamic-floor framing, hard-code constants, use proxies silently, or rescue placeholder Phase 3. |

## v2.1 Patch Note

This v2.1 package integrates the Claude audit response: explicit valid 0–2 compute-eligible Phase 2A outcome, anti-gaming rule for the N≥3 threshold, and conflicting-source tie-break guidance. See `14_CLAUDE_AUDIT_RESPONSE_INTEGRATION.md`.
