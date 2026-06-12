# 02 — Deep Research Prompt: Real ΔE Grounding

## Purpose

Use this prompt before implementation. The goal is to decide whether real ΔE can be defensibly defined from BMR/metabolic-rate data for the already named species and comparison taxa.

This is not a coding prompt.

---

## Execution metadata

| Field | Recommendation |
|---|---|
| Estimated human setup time | 5–10 minutes |
| Estimated model run time | One bounded deep-research session |
| Model | Claude Sonnet high, ChatGPT deep research, or equivalent; no Fable needed initially |
| Effort | High reasoning for source evaluation; not max unless first pass fails |
| Thinking/reasoning | Yes, but ask for concise conclusions and cited evidence, not long visible chain-of-thought |
| Token risk | Medium; keep source set narrow |
| Tools required | Web/literature search, access to AmphiBIO, EltonTraits, TimeTree, possibly Google Scholar/PubMed |
| Cost caveat | Do not ask for full paper drafting or broad literature review yet. |

---

## Copy-paste research prompt

You are performing a narrow scientific feasibility and grounding review for an early-stage research project called **EME Phase 2 — Real ΔE Grounding**.

Do **not** build code yet. Do **not** expand the project. Do **not** write a paper. Your job is to determine whether the next implementation step is scientifically defensible.

### Project context

The project began as a thermodynamic/evolutionary irreversibility analogy inspired by ritonavir polymorphism, but that original framing has been superseded. The active framework is **Epistatic-Metabolic Entanglement / bypass-cost**, where apparent reversals may involve retained ancestral hardware plus secondary bypass structures rather than clean deletion/reversion.

The current repo contains:

- Phase 1 qualitative bypass table.
- A Phase 2 data-pull skeleton.
- A Phase 3 CNE simulation that used placeholder ΔE values `[0.1, 0.25, 0.5, 0.75, 1.0]`.

The existing Phase 3 sigmoid result must be treated as a **methodological artifact**, not a confirmed biological finding.

### Species in scope

Use only these target species unless documenting a data gap:

- `Crocodylus porosus`
- `Alligator mississippiensis`
- `Heterocephalus glaber`
- `Dermochelys coriacea`
- `Thunnus thynnus`

Comparison/outgroup taxa already named:

- `Gallus gallus`
- `Mus musculus`

### Research questions

1. What is the most defensible way to define ΔE from real metabolic-rate data for this project?
   - BMR ratio?
   - log ratio?
   - normalized difference?
   - ancestral-state estimate vs. current estimate?
   - species vs. outgroup comparison?
   - body-mass-normalized metabolic rate?

2. Are AmphiBIO, EltonTraits, and TimeTree suitable sources for the required taxa?
   - Which species have direct records?
   - Which fields are usable?
   - What units and normalization issues exist?
   - What taxonomy-name mismatches should be expected?

3. What data gaps or methodological risks should be documented before implementation?

4. Does Dubiner/Cubo 2026 substantially overlap with the EME framing, or is EME plausibly complementary?
   Compare:
   - mechanism,
   - evidence base,
   - claim type,
   - novelty boundary,
   - whether EME should avoid public novelty claims until real ΔE results exist.

5. Are there credible counterexamples where lineages cleanly lost costly metabolic/physiological traits without retaining ancestral hardware or building bypass structures?
   Keep this narrow. The goal is to identify whether the current “bypass, not reversal” framing has obvious falsifying examples.

### Constraints

- Do not revive the original closed-system “thermodynamic floor” framing.
- Do not treat the placeholder Phase 3 sigmoid result as evidence.
- Do not expand the species list unless a named species lacks usable data; even then, recommend alternatives rather than silently substituting.
- “No threshold found” must remain a valid result.
- Prioritize correctness, auditability, scope control, and cited evidence.

### Required output

Return exactly this structure:

1. Executive verdict
2. Recommended ΔE definition
3. Source viability table
4. Species/data coverage table
5. Data gaps and risks
6. Dubiner/Cubo 2026 differentiation
7. Counterexample risk
8. Whether implementation should proceed
9. Exact implementation guidance for a coding agent
10. Claims that must remain unverified until after the real-ΔE rerun

### Important

If you cannot support a recommendation with sources, say so. Do not infer a formula just because the project needs one.
