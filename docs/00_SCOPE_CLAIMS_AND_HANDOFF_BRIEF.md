00_SCOPE_CLAIMS_AND_HANDOFF_BRIEF.md

Purpose

This document is the top-level scope and context handoff for the EME (Epistatic-Metabolic
Entanglement) project control package, specifically for the Phase 2 “real ΔE grounding” step.

It summarizes the conversation-derived project scope, findings, assumptions, claims,
constraints, risks, and open questions so that another LLM, coding agent, auditor, or
collaborator can understand the current state without needing the full original conversation.

This is not an execution task file.
This is the framing document that all later task files should obey.

-----

Project Summary

Project Name

EME Phase 2 — Real ΔE Grounding

One-Sentence Description

Replace the placeholder ΔE scale (five hand-picked constants, 0.1-1.0) in the Phase 3 CNE
dependency-accumulation simulation with real basal-metabolic-rate-derived ΔE values for the
species already named in the Phase 1 bypass table, then re-run the simulation honestly and
report whatever shape results — including “no threshold” as a fully acceptable outcome.

Practical Goal

Produce a version of the Phase 3 result (sigmoid fit, linear fit, lock-in percentages) that is
driven by real biological ΔE values instead of arbitrary constants, so the headline claim
(“bypass complexity / lock-in scales sigmoidally with ΔE”) can be evaluated against actual
data for the first time. This does not presume that a sigmoid or threshold exists; a failed compute-eligibility gate, a tiny usable-N result, or a clean null/no-threshold outcome is valid evidence about the limits of the current package rather than a failure to be hidden.

Strategic Goal

This is the gating step for the entire EME research direction. The underlying question (why
are some metabolic transitions effectively irreversible?) is live in the literature as of
Feb 2026 (Dubiner). Whether THIS specific framing (bypass complexity as a function of real ΔE)
is a contribution depends entirely on what happens when real numbers replace the placeholder
scale. Until this runs, Phase 3’s current numbers cannot be cited, published, or used to argue
novelty.

Current Stage

Phase 1 (qualitative bypass audit): complete, real citations, sampling caveat noted below.
Phase 2 (real ΔE data pull): not run — phase2_data_pull.py exists as a skeleton only.
Phase 3 (CNE simulation): run once on placeholder ΔE values; result assessed as a
statistical/methodological artifact, not a finding (see Finding 2).

-----

Conversation Context

This project scope was derived from a multi-session discussion:

- Original idea (May 7, 2026): ritonavir polymorphism as an analogy for evolutionary
  irreversibility, framed as a “thermodynamic floor” (v0.1 outline document).
- Gemini Deep Research peer review correctly identified a “physics imperialism” flaw in the
  thermodynamic-floor framing (closed-system thermodynamics doesn’t transfer to open
  biological systems) and proposed the EME / “bypass” reframe: apparent reversals
  (e.g., crocodilian ectothermy) are actually retained ancestral hardware plus an added
  bypass structure, not true reversals.
- Phase 1+2 were run as a combined pass/go gate using real literature searches across four
  species (crocodilians, naked mole-rat, leatherback turtle, bluefin tuna/mako shark) —
  returned “green” with real citations.
- Phase 3 (CNE dependency-accumulation simulation) was built and run in the same session,
  producing a “sigmoid threshold at ΔE≈0.554, ratio 253,988x” headline result, packaged as
  eme_project_v03.zip.
- A follow-up independent audit (this package’s origin) re-examined Phase 3’s code directly,
  ran a sensitivity check, and found the headline ratio is dominated by an arbitrary internal
  constant and a near-zero-residual division artifact — not a robust effect size — and that
  the ΔE axis was never connected to real biology (Phase 2 was never run).
- A fresh novelty check found Dubiner (Journal of Thermal Biology, Feb 2026) asks essentially
  the same question via a different mechanism (pathogen/fever lock-in vs. epistatic-metabolic
  entanglement), citing the same crocodilian source (Seymour et al. 2004) as this project’s
  Phase 1 table.

-----

Core Problem Being Solved

The project is trying to determine whether “bypass complexity scales with the energetic
magnitude (ΔE) of a metabolic transition” is a real, testable pattern in evolutionary
biology, by replacing a placeholder ΔE scale with real basal-metabolic-rate-derived values
for a small set of species and re-running an existing simulation against those real values.

-----

Scope

In Scope

- Pull real metabolic/divergence data for the species already named in
  species_bypass_table.csv (Crocodylus porosus, Alligator mississippiensis,
  Heterocephalus glaber, Dermochelys coriacea, Thunnus thynnus) plus the outgroup pairs
  already specified in phase2_data_pull.py (Gallus gallus, Mus musculus), via AmphiBIO,
  EltonTraits, and TimeTree.
- Define ΔE formally as a ratio or difference of ancestral vs. current basal metabolic rate
  (BMR), using ancestral-state estimates where available; document the formula explicitly.
- Re-run cne_simulation.py with the real ΔE values substituted for the placeholder
  [0.1, 0.25, 0.5, 0.75, 1.0] array.
- Re-run the linear vs. sigmoid fit on the real-ΔE results and report the outcome as-is,
  including a sensitivity check on the lock-in threshold constant (the same check that
  exposed the artifact in the placeholder run).
- Update the Claim Registry in CONTROL_PACK.md to reflect whatever the real-ΔE result
  actually shows.

Out of Scope

- Rewriting or re-litigating Phase 1’s biology (the four-species bypass evidence stands on
  its own merits).
- Expanding the species list beyond what’s already named in phase2_data_pull.py.
- Drafting any methods paper or external-facing claim until Phase 2 results are in hand.
- Changing the CNE simulation’s core architecture beyond swapping the ΔE input array —
  no new model families, no new “complexity scoring” rubric.
- Any framing that revives the original v0.1 thermodynamic-floor language (already
  superseded by the EME/bypass reframe).
- Searching for additional adjacent literature beyond confirming/refuting the Dubiner
  differentiation (that is a separate, smaller task — see Open Questions).

Non-Negotiable Constraints

- Do not relabel the Phase 3 placeholder-ΔE result as “CONFIRMED” — it stays labeled as an
  artifact/methodological note regardless of what the real-ΔE run shows.
- Do not report a “ratio” style headline number without also reporting its sensitivity to
  the lock-in threshold constant and the epsilon used in the division.
- “No threshold found” with real ΔE is a valid, reportable, even useful result — it must not
  be suppressed or reframed as a near-miss.
- A Phase 2A outcome where 0–2 species are compute-eligible (for example, because ectotherm temperature-correction or exact-species metabolic data are unavailable) is also a valid and expected result, not a failure of this package.
- Preserve the existing claim-tagging conventions (CONFIRMED / SIMULATED / QUALITATIVE /
  ASSESSED NOVEL, etc.) used in CONTROL_PACK.md.
- If AmphiBIO/EltonTraits/TimeTree don’t have clean entries for one of the five species,
  document the gap explicitly rather than substituting an estimated value silently.

-----

Key Findings

Finding 1: Phase 1’s bypass pattern is real but sample-selected

Summary:
The four-species bypass evidence (foramen of Panizza in crocodilians, retained UCP1 in naked
mole-rats, gigantothermy + countercurrent heat exchange in leatherbacks, retia mirabilia in
tuna/mako) is accurate and well-cited.

Evidence / Reasoning:
Citations check out against Seymour 2004, Royal Soc 2015, BMC Genomics 2020, and PNAS 2026.
However, all four species were selected specifically because they are the textbook examples
of “didn’t cleanly revert” — no search was performed for counterexamples (lineages that did
cleanly shed a costly trait without a bypass).

Confidence:
High (for the cited facts) / Medium (for the “zero clean deletions” framing, due to selection
bias in the sample).

Implication:
Phase 1 supports “the pattern exists in known cases” but does not yet support “the pattern is
general.” This does not block Phase 2, but should not be oversold as a falsification test.

-----

Finding 2: Phase 3’s headline result is a methodological artifact, not a finding

Summary:
The “sigmoid threshold at ΔE≈0.554, ratio 253,988x” result is an artifact of (a) an
unvalidated placeholder ΔE scale, (b) model equations that bake ΔE into both governing terms
such that an S-shaped output is close to guaranteed by construction, and (c) a ratio metric
(linear residual / (sigmoid residual + 1e-9)) computed on 5 points with a 3-parameter sigmoid,
which is dominated by an epsilon term.

Evidence / Reasoning:
A sensitivity check varying only the lock-in threshold constant (0.45 / 0.50 / 0.55) moved
the “discovered” ΔE* from 0.519 to 0.593 and the ratio from 14,140x to 30,380,388x — over
three orders of magnitude — while the code’s pass/fail verdict (“CONFIRMED” if ratio > 5)
did not change anywhere in that range.

Confidence:
High.

Implication:
The 0.554 / 253,988x numbers must not be cited, published, or used as evidence of anything
about real organisms. The underlying model may still be worth a standalone methods note about
CNE dependency-accumulation dynamics in general, separate from any biological claim.

-----

Finding 3: The underlying question is live; this specific framing’s novelty is unverified

Summary:
Dubiner (Journal of Thermal Biology, Feb 2026) asks essentially the same question — why is
endothermy effectively irreversible — via a different mechanism (pathogen/fever lock-in),
citing the same crocodilian source used in this project’s Phase 1 table. A 2020 Trends in
Ecology & Evolution piece already frames endothermy via metabolic-network-complexity logic
close in spirit to EME. CNE itself (Stoltzfus 1999) was formulated specifically to explain
dependency-driven irreversibility.

Evidence / Reasoning:
Direct literature search (this audit), cross-checked against the May 21 novelty pass.

Confidence:
Medium.

Implication:
“Complementary, not duplicate” (the existing control-pack flag) is plausible but currently an
assertion, not an argument. A short explicit differentiation against Dubiner specifically is
needed before any novelty claim is made publicly — though this is a smaller task than Phase 2
and can follow it.

-----

Claims Register

|Claim                                                                                                         |Type                                          |Evidence                                                  |Confidence                  |Risk if Wrong                                                |Action Needed                                       |
|--------------------------------------------------------------------------------------------------------------|----------------------------------------------|----------------------------------------------------------|----------------------------|-------------------------------------------------------------|----------------------------------------------------|
|Crocodilians/naked mole-rat/leatherback/tuna retained ancestral high-energy hardware + added bypass structures|Fact                                          |Seymour 2004, Royal Soc 2015, BMC Genomics 2020, PNAS 2026|High                        |Low — well-cited                                             |None                                                |
|“Zero clean deletions” generalizes beyond these 4 species                                                     |Hypothesis                                    |None — no counterexample search performed                 |Low                         |Medium — overclaiming generality                             |Defer; out of scope for Phase 2                     |
|Bypass complexity scales sigmoidally with ΔE                                                                  |Hypothesis                                    |Phase 3 sim on placeholder ΔE only                        |Low                         |High — currently the headline claim and currently unsupported|Verify via Phase 2 (this brief)                     |
|ΔE≈0.554 / ratio 253,988x is a meaningful result                                                              |Design Claim (currently mislabeled as finding)|Sensitivity check shows artifact                          |High (that it’s an artifact)|High if cited externally                                     |Do not cite; supersede via Phase 2                  |
|EME framing is “complementary, not duplicate” of Dubiner 2026                                                 |Assumption                                    |Asserted in CONTROL_PACK, not argued                      |Medium                      |Medium — novelty claim could collapse                        |Write explicit differentiation (separate small task)|

-----

Important Considerations

Technical Considerations

- AmphiBIO and EltonTraits use specific scientific-name formats; species-name matching
  (Crocodylus porosus, Heterocephalus glaber, etc.) needs to be verified against each
  dataset’s taxonomy before any join.
- ΔE has never been formally defined for this project beyond “energy magnitude of the
  transition” — this brief requires it be defined as an explicit formula (e.g., ancestral
  BMR / current BMR, or normalized difference) before Phase 3 is re-run, so the result is
  reproducible.
- TimeTree’s API (used in phase2_data_pull.py) may be rate-limited or return incomplete
  pairwise data for some species pairs — a fallback to published divergence estimates from
  literature should be documented if so.
- cne_simulation.py’s other constants (mutation rate 0.003, lock-in threshold 0.50,
  deps-scaling 0.015, exponent 1.5) remain unvalidated against any biological process and
  should be flagged as such in the re-run’s output, even if untouched.

Product / User Considerations

- This is a personal research-portfolio project; the audience is “another LLM, a future
  collaborator, or a journal reviewer,” not an internal team.
- What makes this useful instead of merely interesting: a real-ΔE result either supports a
  citable, defensible prediction, or honestly closes this branch and redirects effort —
  both are valuable outcomes for the portfolio.

Agent Execution Considerations

- The agent should work from this document before touching phase2_data_pull.py or
  cne_simulation.py.
- The agent should report the ΔE formula it intends to use before pulling data, since this
  is the single most consequential undocumented decision in the project.
- The agent should not attempt to “rescue” the 0.554/253,988x framing — if real ΔE produces
  a different shape (or none), that is the result.
- The agent should avoid adding new files, new frameworks, or new species beyond what’s
  already named, per Out of Scope.

-----

Decisions Already Made

|Decision                                                                              |Reason                                                                                    |Reversible?                 |Notes                                                          |
|--------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------|----------------------------|---------------------------------------------------------------|
|Reframe from “thermodynamic floor” to “Epistatic-Metabolic Entanglement / bypass-cost”|Gemini peer review identified a closed-system-thermodynamics error in the original framing|No (without redoing Phase 1)|Crocodile-as-bypass reframe is the strongest part of this pivot|
|Combine Phase 1+2 as a single pass/go gate before Phase 3                             |Phase 1 is qualitative/categorical, Phase 2 is quantitative on the same dataset           |Partially                   |Phase 1 ran; Phase 2 (quantitative) did not                    |
|Build Phase 3 as a pure-Python CNE simulation (numpy/scipy, no GPU needed)            |300-500 lines, runs in ~90s on CPU at n=3000                                              |Yes                         |Architecture is fine; only the ΔE input needs replacing        |

-----

Open Questions

|Question                                                                                                  |Why It Matters                                                                       |Suggested Way to Resolve                                                                                   |
|----------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------|
|How should ΔE be formally defined from real BMR data (ratio vs. difference, which ancestral baseline)?    |This single decision determines what “0.554-equivalent” would even mean in real units|Research — pick a defensible definition from bioenergetics literature before pulling data                  |
|Does a threshold/sigmoid shape survive when real ΔE replaces the placeholder scale?                       |This is the actual test of the headline hypothesis                                   |Run Phase 2 + re-run Phase 3 (this brief’s core task)                                                      |
|Is the EME framing genuinely complementary to Dubiner 2026, or does it overlap more than currently argued?|Determines whether any novelty claim is defensible                                   |Research — direct comparison of the two mechanisms against the same evidence base                          |
|Are there clean-deletion counterexamples that would falsify “bypass, not reversal” as a general pattern?  |Affects how strongly Phase 1 can be cited                                            |Research — targeted literature search for lineages that lost costly traits without retaining/bypassing them|

-----

Risks

|Risk                                                                                                                         |Likelihood|Impact|Mitigation                                                                                           |
|-----------------------------------------------------------------------------------------------------------------------------|----------|------|-----------------------------------------------------------------------------------------------------|
|Real ΔE data shows no threshold/sigmoid (linear, flat, or noisy)                                                             |Medium    |Medium|Report honestly; reframe as “no evidence of non-linear scaling” — still a valid, citable null result |
|Species-name or data-availability gaps in AmphiBIO/EltonTraits/TimeTree for one or more of the 5 species                     |Medium    |Medium|Document gaps explicitly; do not substitute estimated values without flagging them as such           |
|Dubiner 2026 (or similar) is found to overlap more than “complementary” once compared directly                               |Low       |Medium|Treat as a finding, not a failure — narrows scope of any future claim rather than killing the project|
|Re-running Phase 3 with real ΔE produces a new “impressive number” that has the same artifact shape (epsilon-dominated ratio)|Medium    |High  |Apply the same sensitivity check (vary the lock-in threshold) to any new result before reporting it  |

-----

Recommended Next Step

The next agent or reviewer should begin by:

1. Reading this file completely.
1. Proposing and documenting an explicit, defensible formula for ΔE from real BMR data.
1. Running phase2_data_pull.py (or its successor) against AmphiBIO, EltonTraits, and TimeTree
   for the five named species plus outgroups, documenting any data gaps.
1. Re-running cne_simulation.py with the resulting real ΔE values substituted for the
   placeholder array, including the lock-in-threshold sensitivity check.
1. Updating CONTROL_PACK.md’s Claim Registry with the real-ΔE result — whatever it is.
1. Only then revisiting the novelty differentiation against Dubiner 2026 (Open Questions, item 3).

-----

How to Use the Rest of This Package

This file defines the project frame for the EME Phase 2 step specifically.

- 01_REPO_AUDIT.md — not yet created; would cover eme_project_v03’s current file structure.
- 02_IMPLEMENTATION_PLAN.md — would formalize the ΔE definition + data-pull + re-run steps
  above into an execution plan.
- 03_VALIDATION_PLAN.md — would specify the sensitivity check as a required step for any
  new headline number.
- 04_RISK_REGISTER.md — see Risks above; promote to its own file if this expands.
- 05_AGENT_EXECUTION_INSTRUCTIONS.md — see Agent Execution Considerations above.
- 06_DELIVERABLES_CHECKLIST.md — not yet created; should include “sensitivity check included”
  as a required item for Phase 3 re-run outputs.

If any later file conflicts with this scope brief, the agent should pause and explicitly
identify the conflict instead of silently choosing one direction.

-----

Handoff Instruction to Another LLM

You are receiving this document as a project scope and findings handoff.

Your job is not to blindly agree with it.
Your job is to preserve the useful context, challenge weak claims, identify missing evidence,
and propose a bounded next step.

Do not expand the project unless expansion is directly justified by the stated goal.

Prioritize:

1. Correctness
1. Auditability
1. Scope control
1. Clear execution
1. Evidence-backed claims
1. Minimal useful implementation

Return your response in this structure:

1. Understanding of the Project
1. Strongest Confirmed Claims
1. Weak or Unverified Claims
1. Risks
1. Recommended Next Step
1. What You Would Not Build Yet
1. Questions That Actually Matter