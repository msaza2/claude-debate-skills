---
name: general-debate
description: Use ONLY when explicitly invoked — "/general-debate", "run a general debate on X", "debate which laptop/parts/recipe/plan", "convene a debate on X" — for ANY topic outside software products: purchases, hardware builds, cooking, travel, plans, life/business decisions. Moderator triages volatile facts, builds a web-researched evidence pack (no guessing — prices, specs, availability are looked up, dated, sourced), instantiates topic-specific expert seats from a domain-agnostic archetype charter, runs them as blind parallel critics with mandatory lane-scoped web research, adversarially verifies findings with refuter agents, and synthesizes a verdict via a generalized precedence ladder. For software products/features/specs/repos prefer software-debate. Never invoke proactively.
---

# General Debate

Layered critique machine for any decision. You are the **moderator**: you hold no substantive opinion — your authority is procedural. Quality comes from four mechanisms, in order of importance: (1) **web-researched evidence grounding**, (2) independent blind critique, (3) adversarial verification of findings, (4) precedence-based synthesis. Debate is a scalpel used only where verified findings genuinely conflict — never an all-vs-all ritual.

## Prime directive — research, don't guess

This skill's topics (products, prices, recipes, plans, rules) live in the changing world, not in a repo you can read. Therefore:

- **Volatile facts MUST be looked up, never recalled.** Volatile = anything that changes or varies: prices, availability/stock, product specs and SKUs, model numbers and revisions, versions, compatibility matrices, release dates, regulations/policies/fees, current recommendations and consensus best practice. Model memory on these counts as `assumed`, no matter how confident it feels.
- **Every looked-up fact carries source + access date.** An undated fact is an unverified fact.
- **Stable knowledge** (physics, arithmetic, food-science fundamentals, technique principles) may come from knowledge, tagged `stable-knowledge` — but when a "stable" belief is decision-critical, verify it anyway; many are folklore.
- **Unobtainable facts are declared, not filled in.** Tag `assumed`, list in gaps, and let the verdict carry the lookup as a pre-commitment test. Silent guessing anywhere in the pipeline is a violation.
- Research tools: WebSearch/WebFetch by default; richer research tooling (e.g. platform-specific search skills) when available and relevant. Prefer independent sources over seller/marketing pages. Retailer sites that 403 WebFetch (Micro Center, Best Buy, Crucial…) usually load fine in the in-app browser (`preview_start`/`navigate` + `get_page_text`) or via their JSON-LD product schema — a direct page read beats a search-index snippet, and when they disagree, the direct read wins.
- **Prices can be store/region-scoped.** A retailer may show different prices per selected store; search indexes may cache one store's price while the bare page shows another. Record the scope qualifier with the price ("no store selected", "store X"), and when two same-day reads disagree, report the split rather than picking one.

## The archetype charter

One charter file lives in this skill's `charters/` directory: `charters/archetypes.md`. It contains global lane/evidence rules plus seven `## SEAT:` sections. These are **seat skeletons, not finished seats** — each defines a jurisdiction and method that you instantiate into the topic at hand:

| Archetype | Jurisdiction |
|---|---|
| craftsman | how this is done well — technique, execution quality, method correctness |
| fit-advocate | the user's actual needs, constraints, and context — right-sizing |
| value-analyst | cost arithmetic — TCO, price-performance, diminishing returns |
| risk-analyst | failure modes, safety, hard constraints, recovery paths |
| evidence-skeptic | claims vs independent reality — source forensics, myth-busting |
| logistics | acquisition and operation practicality — availability, timeline, dependencies |
| alternatives-scout | the frame itself — better options outside the stated question |

Resolve the absolute path of this SKILL.md's directory and pass the full charter path to agents.

## Layer 1 — Scope the case

One `AskUserQuestion` only for what's genuinely unknown (skip anything already stated):

- **Target & shape** — every debate has one of three shapes; name it, the verdict format depends on it:
  - `evaluate` — judge one thing ("is this recipe/build/plan sound").
  - `compare` — rank N named options ("which of these three laptops").
  - `compose/scrutinize` — improve a plan or list with parts ("scrutinize my parts list", "build me a menu").
- **Decision at stake** — what question must the debate answer, with what commitment behind it ("buy this week or wait", "cook this for 12 guests Saturday"). No decision = noise; insist on one.
- **Depth** — `quick` (seats run with research, no refuter layer, verdict from merged findings) or `standard` (full pipeline, default) or `deep` (full pipeline + 2 refuters per finding + completeness critic). At `quick`, the verdict's Coverage & integrity section MUST state that no refuter pass ran and severities are seat-claimed + moderator-merged only.
- **Context pack** — budget cap, deadline, region/country (prices, availability, and rules are region-scoped — ask if material and unknown), skill level, existing equipment/inventory, hard preferences and exclusions (allergies, dealbreakers), prior research the user already did.

## Layer 2 — Evidence & research pack (moderator builds, BEFORE spawning)

Grounding beats topology — one seat arguing from a dated spec sheet beats five arguing from vibes. Assemble once, give identically to every seat:

- **Volatility triage.** List the facts the decision hinges on; mark each `volatile` (must look up) or `stable`. This table drives the baseline sweep and tells seats what they may not assume.
- **Baseline web sweep.** Look up the volatile facts every seat will need — current prices in the user's region, official spec sheets, availability, version/release status. Record as a fact table: `fact | value | source | access date`. Don't boil the ocean: seats do their own lane research; the baseline covers shared ground truth.
- User-provided material (lists, links, photos, prior research) → read it; include paths/excerpts. User's context pack verbatim.
- What could NOT be gathered — listed explicitly, so seats mark related findings `assumed` instead of guessing.
- **Timestamp every observation.** Prices and stock move mid-week; the pack states its own access dates so a later reader knows when truth was sampled.
- **Separate observed fact from moderator interpretation.** Write "listing shows 2×8GB DDR5-5600, $73, Newegg, 2026-07-26" (fact), not "RAM is cheap right now" (interpretation) — seats inherit your framing errors and build findings on them.
- **Never assert absence without checking** — "no AM5 board under $120 exists", "no gluten-free substitute works" are lookup-able claims, not pack facts, until searched.

## Layer 3 — Roster (instantiate archetypes into seats)

Pick ONLY archetypes with real jurisdiction over the decision — fixed rosters make irrelevant agents manufacture findings. For each selected archetype, write a **topic instantiation**: a seat name plus a 2–4 line brief saying what the jurisdiction means HERE and what its priority research targets are (e.g. craftsman → "PC-builder seat: socket/RAM/PSU compatibility practice, thermal design, assembly order; research current build guides for this CPU generation").

- Typical counts: quick → 3–4 seats; standard → 4–6; deep → 6–8.
- **Custom specialist seats:** up to 2 when the topic demands genuine specialty no archetype covers (battery chemistry, fermentation, visa rules). Custom seats get a written brief in the same shape and are bound by the charter's global lane/evidence rules and the finding contract.
- List selected seats + one-line reason each; list excluded-but-borderline archetypes with why — silent omission hides blind spots. User can override the roster.

## Layer 4 — Blind critique (parallel, one agent per seat)

Spawn all selected seats **in a single message** (general-purpose agents, web access assumed). Each prompt contains:

1. "Read `<charter path>`. Embody ONLY `## SEAT: <archetype>` — plus the charter's global lane and evidence rules, which bind you. Your topic instantiation: `<brief>`. Other jurisdictions are covered by other agents; do not drift into them."
2. The evidence & research pack + decision at stake + shape.
3. **Mandatory research clause:** "You MUST run your own lane-scoped web research before finalizing findings. Do not guess volatile facts (prices, specs, availability, versions, rules, current consensus) — look them up; every such claim in your findings carries source + access date or is tagged `assumed`. A finding built on an assumed volatile fact must say so in its evidence line."
4. "You have NOT seen other seats' output. Critique independently."
5. Output contract — findings only, no praise, no preamble:

```
FINDING <seat>-<n>
severity: blocker | major | minor | idea
claim: <one sentence, specific — name the item/step/number, not the vague area>
why-it-hurts: <consequence for the user's decision or outcome>
evidence: <pack item or own lookup> [tier: verified-web (source, date) | user-provided | stable-knowledge | assumed]
fix: <concrete action, substitution, or the cheapest lookup/test that closes the gap>
```

6. "If you genuinely have little to say, return 1–2 findings or `NO-MATERIAL-FINDINGS` + one sentence why. Padding is a violation — a short honest return beats manufactured findings."

## Layer 5 — Mechanical merge (no agent)

You do this yourself:

- Dedup near-identical claims across seats; multi-seat independent hits are STRONGER — tag `corroborated-by: [seats]`, keep highest severity.
- Discard lane violations (seat asserting outside its jurisdiction — reroute to the owning seat's pile or drop).
- **Enforce the no-guess rule:** a finding whose decisive evidence is `assumed` on a volatile fact gets downgraded one severity step OR converted into a verification task for the verdict's tests section — never carried forward as if verified. Evidence-tier inflation (assumed presented as verified) → downgrade or drop.
- **Moderator fact-check:** before spending a refuter, ground-truth any claim you can settle with one cheap search (a current price, a spec line, stock status). A finding killed by one lookup dies here for free — log it as refuted-at-merge.
- Corroboration count strengthens confidence a defect EXISTS, not its severity — seats sharing the same surface co-observe the same shallow symptom.
- Sort: blockers first, then majors, then rest.

## Layer 6 — Adversarial verification (standard/deep; replaces all-vs-all debate)

For every surviving **blocker and major**: spawn refuter agents in parallel (1 per finding at standard, 2 at deep — second refuter gets a different lens: "wrong on facts" vs. "true but immaterial to the decision").

Refuter prompt: "Try to kill this finding: `<finding>`. Evidence pack: `<pack>`. **Attack by lookup first — a fresh search that contradicts the finding beats any argument.** Attack the factual basis, the severity, or the materiality to the decision at stake. Return `REFUTED` (+why), `DOWNGRADED` (+new severity +why), or `STANDS` (+what you tried). You earn nothing by agreeing — a lazy STANDS is a failure; a wrong REFUTED kills a real issue."

Attack angles to bake into every refuter prompt:

- **Stale-data check:** is the finding's source dated? Prices, stock, versions, and rankings rot in weeks — a finding citing last year's price or a superseded model revision dies on a fresh lookup.
- **Wrong-variant check:** does the claim hold for the EXACT variant at stake? Same product name ships in different revisions, regional SKUs, capacities, and formulations (a GPU name with two VRAM configs, an ingredient that differs by country). Verify the finding tested the user's variant, not a namesake.
- **Source-quality check:** is the evidence an independent test, or marketing, affiliate content, astroturfed reviews, or ten articles all citing one press release? Reweigh accordingly.
- **Already-handled check:** does the user's stated plan, context pack, or existing equipment already mitigate this? Seats under-read the context pack the way they under-read codebases.
- **Fix-sanity check:** attack the proposed fix too — a fix that's unavailable, costs more than the problem, or violates the user's constraints discredits the severity even when the observation is real.
- **Materiality check:** true, but does it change the decision at stake at all?

Apply verdicts: refuted findings drop (logged), downgraded findings re-rank. Exception: an unrecoverable-harm safety blocker (fire, foodborne illness, legal exposure, injury) survives refutation unless the refuter shows the *standard is misapplied* — severity taste doesn't kill those. Minors/ideas pass through unverified (cost > value).

At **deep**, add one completeness critic in parallel with refuters: "Given this decision, evidence pack, and these findings — what did every seat miss? Un-run lookup, unexamined assumption, absent specialty?" Its output feeds the verdict's gaps section (or a supplemental seat run if it names a missing jurisdiction).

## Layer 7 — Conflict resolution (only where needed)

Find genuine conflicts among surviving findings (recommend opposite actions, or a fix for one violates another). Usually 0–3. For each, spawn ONE head-to-head agent: both findings + both seats' charter sections + evidence pack → "Resolve: adjudicate with the evidence, or synthesize a path satisfying both, or declare a true value conflict." Never all-vs-all.

Precedence ladder for adjudication (and for your own final ranking):

1. **Safety / irreversible harm / legality** — stands unless the standard is misapplied; cheapest compliant path attached.
2. **Hard constraint violation** — physical incompatibility, allergy/exclusion, hard budget cap, deadline arithmetic that doesn't close. Spec or arithmetic shown, verified against the exact variant.
3. **Verified empirical evidence** — independent tests, benchmarks, measured results, dated lookups. Beats every opinion.
4. **Quantified economics** — price-performance or cost arithmetic, shown. Beats preference and craft.
5. **Practicality** — effort, availability, maintenance burden; reshapes scope of whatever survives 1–4.
6. **Taste/preference** — loses to all above; surviving taste conflicts go to the operator.

Evidence-free deadlock → resolution is the cheapest lookup or real-world test that settles it. True value conflict → don't fake a resolution; escalate to operator with both cases stated.

## Layer 8 — Verdict

```markdown
# DEBATE VERDICT — [target]
**Decision at stake:** [question] → **Answer:** [direct answer + one-line why]
```

The answer block depends on shape:

- `evaluate` → direct verdict on the thing + conditions under which it flips.
- `compare` → ranking table (`option | verdict | decisive findings`), a named winner, and switch conditions ("pick B instead if budget rises $100 / if cooking for >8").
- `compose/scrutinize` → the revised plan/list itself, each change annotated with its source finding; unchanged items confirmed as reviewed.

Then the common sections:

```markdown
## Confirmed blockers (survived adversarial verification)
- [finding] — [seat(s)] — [fix / exit criteria]
## Confirmed majors
- [finding] — [seat(s)] — [fix]
## Resolved conflicts
- [A vs B] — [resolution + precedence rule applied]
## Tests before commitment
- [assumption or unverified volatile fact] — [cheapest lookup/test + cost]
## Escalations (operator must decide)
- [conflict] — [case A vs case B, one line each]
## Minors & ideas (unverified)
- [compact list]
## Key facts used (spot-checkable)
- [fact | value | source | access date] — the decision-critical rows only
## Coverage & integrity
- Seats run: [list] · Excluded: [list + why]
- Refutation: X findings verified → Y stood, Z refuted, W downgraded
- Research gaps: [facts that stayed assumed / couldn't be gathered]
- Staleness: prices/stock checked [date] — revalidate if deciding after [horizon]
```

Direct answer required — never a mushy "consider everything" list. A finding refuted is reported as refuted; don't resurrect it softened.

## Rules

- Moderator holds no opinion. Procedure, lanes, evidence tiers, precedence — nothing else.
- The no-guess rule binds every layer: moderator pack, seat findings, refuter attacks, verdict. Volatile fact ⇒ lookup with source + date, or `assumed` tag + gaps entry.
- Evidence pack BEFORE spawning, identical for all seats; enforcement of "no vibes findings" happens at merge.
- Round-1 blindness is sacred — never leak one seat's output into another's prompt. Blindness applies to other seats, not to the web: independent research is required, shared conclusions are forbidden.
- Refuters must genuinely attack, not perform review-theater; lookup beats argument.
- This skill researches and decides; it does not purchase, book, order, or implement. Offer execution as a separate opted-in follow-up.
- Regulated-advice domains: for medical, legal, or investment decisions the debate delivers research, frameworks, and questions-to-ask — not personalized directives; say "consult a professional" plainly. Personalized investment advice is refused outright.
- Software products, features, specs, repos → route to `software-debate` instead (its charter library is purpose-built for that).
- Archetype expertise lives in `charters/archetypes.md` — update it there, never in this file.

## Self-improvement (standing rule)

This skill is self-improving. After every debate (verdict delivered, or aborted), spend one minute on friction: an archetype instantiation that came out too vague to constrain its seat, a volatile-fact class the triage missed, a refuter attack angle that killed findings and deserves baking into the template, a verdict line that misled a reader. If the lesson generalizes, fix it in the same session — pipeline/mechanics here in SKILL.md, seat method in `charters/archetypes.md` (never seat content in this file). Rules:

- **Process-agnostic only.** Never bake in one debate's topic, products, prices, or verdict. Case-specific lessons belong in that conversation; only reusable *method* lands here.
- Fix the doc, don't grow it: prefer correcting the wrong line over appending a caveat; prune superseded text. Date an observation only when the date disambiguates.
- Keep the mechanism ranking intact — a fix that strengthens research grounding outranks one that adds topology (more seats/refuters is rarely the answer).
