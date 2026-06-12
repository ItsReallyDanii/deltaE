# 07 — Model and Cost Guidance v2

## Purpose

Choose the cheapest model strategy that still protects scientific and implementation quality.

---

## Current recommendation

Do not use Fable/Max for Phase 2A implementation.

Use the expensive models only when:

- Sonnet/Gemini/ChatGPT disagree on scientific interpretation,
- the package is being externally audited before public presentation,
- Phase 3 produces an ambiguous result,
- the project grows beyond the current small repo.

---

## Task-by-task model guidance

| Task | Recommended model | Effort | Notes |
|---|---|---:|---|
| Update control package | ChatGPT strong reasoning / Sonnet normal | Medium | Already done in v2. |
| Claude audit of v2 package | Sonnet 4.6 first | High or normal-high | Best value for long document audit. |
| Second-opinion critique | Opus 4.8 | Low/medium | Use only if credit budget is valid and audit needs sharper judgment. |
| Phase 2A coding | Claude Code Sonnet / normal coding model / Hermes | Medium | Bounded implementation; no Fable needed. |
| Final claim audit | Sonnet high or Opus low/medium | High for Sonnet; low/medium for Opus | Only after outputs exist. |
| Open-ended autonomous repo work | Avoid | N/A | Too easy to burn credits and create churn. |

---

## Claude recommendation for next step

For the “original Claude creator” audit:

### Preferred

```text
Claude Sonnet 4.6
Mode: LLM chat or Claude web chat
Effort: High if available; otherwise normal
Task: Audit only, no building/code execution
Attachments: v2 ZIP + maybe only key files if upload limit is tight
```

Why:

- The task is document audit and scope discipline, not heavy code generation.
- Sonnet is cheaper than Opus and strong enough to catch package issues.
- You can reserve Opus for a second-pass critique if Sonnet finds major conflict.

### Use Opus 4.8 only if

```text
You want a second-opinion critic after Sonnet.
You have active credits you are comfortable spending.
You keep effort low/medium and forbid broad rewriting.
```

Recommended Opus setting:

```text
Claude Opus 4.8
Effort: Low or medium
Task: Critique the v2 package and Sonnet audit; identify top risks and missing gates.
Do not rewrite the package unless explicitly asked.
```

### Avoid

```text
Sonnet 4.6 Max for long autonomous repair
Fable 5 for this stage
Opus high/max for package drafting
```

---

## Cost/risk notes

- Document audits can become expensive when the model reprocesses the same attachments repeatedly.
- Prefer one bounded prompt over iterative “thoughtful” back-and-forth.
- Ask for a risk list and patch recommendations, not a full rewrite.
- If a model suggests expanding the species list, treat that as future work, not Phase 2A.

---

## Suggested Claude attachment set

Best upload set:

```text
EME_Phase2_Real_DeltaE_Control_Package_v2_20260612.zip
```

If Claude struggles with ZIPs, upload only:

```text
00_SCOPE_CLAIMS_AND_HANDOFF_BRIEF.md
README_CONTROL_PACKET.md
03_IMPLEMENTATION_PLAN.md
04_VALIDATION_PLAN.md
05_AGENT_EXECUTION_INSTRUCTIONS.md
11_GEMINI_AUDIT_INTEGRATION.md
12_CLAUDE_AUDIT_PROMPT.md
13_PHASE2_DATA_SCHEMA.md
```

Optional:

```text
research_memos/PERPLEXITY_UPDATED_DEEP_RESEARCH_MEMO.md
research_memos/GEMINI_AUDIT_IMPLEMENTATION_PLAN.md
```

Do not upload full unrelated chat history unless Claude specifically needs provenance.
