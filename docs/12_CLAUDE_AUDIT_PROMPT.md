# 12 — Claude Audit Prompt for v2 Control Package

## Purpose

Use this prompt when sending the v2 control package back to Claude for an external audit.

Recommended first model: **Claude Sonnet 4.6** with high/normal-high effort if available.  
Recommended escalation: **Claude Opus 4.8 low/medium** only for a second-pass critique if credit budget is acceptable.

---

## Attachments

Preferred:

```text
EME_Phase2_Real_DeltaE_Control_Package_v2_20260612.zip
```

If ZIP upload is unreliable, attach these files:

```text
00_SCOPE_CLAIMS_AND_HANDOFF_BRIEF.md
README_CONTROL_PACKET.md
03_IMPLEMENTATION_PLAN.md
04_VALIDATION_PLAN.md
05_AGENT_EXECUTION_INSTRUCTIONS.md
09_CLAIM_REGISTRY_PATCH_GUIDE.md
11_GEMINI_AUDIT_INTEGRATION.md
13_PHASE2_DATA_SCHEMA.md
```

Optional support memos:

```text
research_memos/PERPLEXITY_UPDATED_DEEP_RESEARCH_MEMO.md
research_memos/GEMINI_AUDIT_IMPLEMENTATION_PLAN.md
```

---

## Copy-paste prompt

You originally helped develop or review the EME project idea. I am now sending you the updated **EME Phase 2 — Real Data Grounding Control Package v2** for audit.

Please do **not** build code yet. Do **not** rewrite the whole package. Do **not** expand the species list. Your task is to audit whether this v2 control package is scientifically and operationally safe to hand to a coding agent or Hermes.

### Critical context

The old Phase 3 result used placeholder ΔE values and produced a sigmoid/threshold claim. That result must remain artifact/provenance only.

The active v2 correction is that the metric should not be treated as true historical transition energy. It should be treated as:

```text
Current Clade-Relative Metabolic Deviation
```

with both:

```text
signed_log_residual
Delta_E_magnitude / delta_E_magnitude
```

The package also prohibits hard-coded allometry constants, silent proxy substitution, and computation from FMR/MMR/DMR/active/postprandial data by default.

### Audit questions

1. Does v2 correctly preserve the useful EME/bypass-cost framing without reviving the old thermodynamic-floor framing?
2. Does v2 correctly demote the placeholder Phase 3 sigmoid result to artifact/provenance only?
3. Is “Current Clade-Relative Metabolic Deviation” the right operational name for the proposed metric?
4. Are the required output files sufficient for Phase 2A source/data verification?
5. Are the quality gates strict enough to prevent silent data corruption?
6. Are the coding-agent instructions too strict, too loose, or correctly bounded?
7. Does the package over-rely on Gemini’s audit anywhere?
8. Are there any missing files, missing fields, or missing validation checks before implementation?
9. Should any part of this package be simplified before handing it to Hermes / Claude Code?
10. Is the next step research, implementation, validation, or another audit?

### Required response format

Return exactly:

1. Overall verdict: safe to hand to coding agent / needs patch / not safe
2. Strongest parts of the v2 package
3. Weakest parts or remaining risks
4. Any wording that still overclaims
5. Any implementation instruction that could cause bad data
6. Files that should be patched before execution
7. Minimal patch recommendations
8. Whether Phase 2A implementation should proceed
9. What you would not build yet
10. Recommended model/effort for the next actual implementation step

Please keep the response audit-focused and bounded. Do not generate a new theory or paper draft.
