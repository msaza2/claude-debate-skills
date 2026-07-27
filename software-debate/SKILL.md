---
name: software-debate
description: Use ONLY when explicitly invoked by name — "/software-debate", "run software debate on X", "debate this design choice", "convene the debate on X". Layered multi-agent critique of a product, feature, spec, design choice, or plan. Moderator picks relevant expert seats from four charter files, runs them as blind parallel critics, adversarially verifies their findings with refuter agents, resolves conflicts head-to-head, and synthesizes a verdict via a precedence ladder. Never invoke proactively.
---

# Software Debate

Layered critique machine. You are the **moderator**: you hold no substantive opinion — your authority is procedural. Quality comes from four mechanisms, in this order of importance: (1) evidence grounding, (2) independent blind critique, (3) adversarial verification of findings, (4) precedence-based synthesis. Debate is a scalpel used only where verified findings genuinely conflict — never an all-vs-all ritual.

## The charter library

Four charter files live in this skill's `charters/` directory. They are knowledge bases, not agents. Each contains delegation-level lane/evidence rules plus `## SEAT:` sections:

| Charter | File | Seats |
|---|---|---|
| Builders | `charters/builders.md` | product-manager, designer, engineer, sre |
| Gatekeepers | `charters/gatekeepers.md` | qa, security, legal-privacy, accessibility |
| Truth Sources | `charters/truth-sources.md` | ux-research, analytics, support-cs |
| Market Reality | `charters/market-reality.md` | sales, marketing, finance |

Resolve the absolute path of this SKILL.md's directory and pass full charter paths to agents.

## Layer 1 — Scope the case

One `AskUserQuestion` only for what's genuinely unknown:
- **Target** — spec, feature, product, design choice, plan, repo, URL, doc.
- **Decision at stake** — what question must the debate answer ("build it or not", "which option", "is this launch-ready", "what must change first"). No decision = noise; insist on one.
- **Depth** — `quick` (seats run, no verify layer, verdict from merged findings) or `standard` (full pipeline, default) or `deep` (full pipeline + 2 refuters per finding + completeness critic). At `quick`, the verdict's Coverage & integrity section MUST state that no refuter pass ran and severities are seat-claimed + moderator-merged only — downstream readers (and any records filed from the verdict) must not cite quick-mode findings as refuter-verified.
- **Context pack** — audience, competitors, constraints, prior evidence, budget, stage of company.

## Layer 2 — Evidence pack (moderator builds, BEFORE spawning)

Grounding beats topology — an agent critiquing a rendered page beats five critiquing a description. Assemble once, give identically to every seat:

- Live URL / local app → screenshot desktop (~1440×900) AND mobile (~390×844); note console errors and whether the primary task completes. Repo with runnable app → use the `run` skill to get it rendered.
- Files/specs/images/PDFs → read them; include paths + key excerpts.
- Repo → relevant source paths, schema, API surface (use a code-graph/index tool first if the project has one).
- User's context pack verbatim.
- What could NOT be gathered — listed explicitly, so seats mark related findings unverified instead of guessing.
- **Timestamp every live observation** (queue counts, git state, health rows) — packs go stale mid-debate, and a seat will cite the stale number as current fact.
- **Separate observed fact from moderator interpretation.** Write "files X exist unrouted" (fact), not "dead code from an older build" (interpretation) — seats inherit your framing errors and build findings on them, and refuters then spend effort correcting the pack instead of the product.
- **Never assert a capability is ABSENT without checking the system's own registry/config** (tool lists, permission profiles, feature flags). "X has no Y access" stated as pack FACT seeds false blockers in every seat that touches it; a scoping rule quoted without its qualifier ("for the morning brief, …") does the same. Quote scope qualifiers verbatim.

## Layer 3 — Roster selection (individual seats, not whole charters)

Pick ONLY seats with real jurisdiction over the decision. Fixed rosters make irrelevant agents manufacture findings to justify their existence — that's noise you pay to filter. Triggers:

| Seat | Include when the decision touches… |
|---|---|
| product-manager | scope, priorities, problem framing, any feature decision (almost always) |
| designer | anything rendered or interacted with |
| engineer | implementation, architecture, data model, API, tech choice |
| sre | scale, uptime, infra cost, deploy/rollback, ops |
| qa | testability, quality bars, release readiness |
| security | auth, personal data, money, new endpoints, file handling, AI agency, third parties |
| legal-privacy | personal data, consent, AI decisions about people, claims/promises, regulated domains |
| accessibility | any human-facing UI |
| ux-research | assumptions about user behavior/comprehension/desire |
| analytics | success measurement, experiments, instrumentation |
| support-cs | flows users could get confused/stuck in; support-load implications |
| sales | B2B selling motion, demos, procurement, competitive position |
| marketing | positioning, naming, launch, segment choice |
| finance | pricing, COGS, unit economics, billing |

Typical counts: narrow UI tweak → 3–4 seats; new feature → 6–8; full product/launch review → 10–14. List selected seats + one-line reason each; list excluded-but-borderline seats with why — silent omission hides blind spots. User can override the roster.

## Layer 4 — Blind critique (parallel, one agent per seat)

Spawn all selected seats **in a single message** (general-purpose agents). Each prompt contains:

1. "Read `<charter path>`. Embody ONLY `## SEAT: <name>` — plus the charter's lane rules and evidence rules, which bind you. Other seats are covered by other agents; do not drift into their jurisdiction."
2. The evidence pack + decision at stake.
3. "You have NOT seen other seats' output. Critique independently."
4. Output contract — findings only, no praise, no preamble:

```
FINDING <seat>-<n>
severity: blocker | major | minor | idea     (gatekeeper seats: block | condition | debt)
claim: <one sentence, specific — name the element/mechanism, not the vague area>
why-it-hurts: <consequence for user/business/system>
evidence: <what in the evidence pack supports this> [tier: verified | inferred-from-source | assumed]
fix: <concrete action — or for gatekeeper seats: standard cited + exit criteria + cheapest compliant path>
```

5. "If you genuinely have little to say, return 1–2 findings or `NO-MATERIAL-FINDINGS` + one sentence why. Padding is a violation — a short honest return beats manufactured findings."

## Layer 5 — Mechanical merge (no agent)

You do this yourself:
- Dedup near-identical claims across seats; multi-seat independent hits are STRONGER — tag `corroborated-by: [seats]`, keep highest severity.
- Discard lane violations (seat asserting outside its jurisdiction — e.g., engineer claiming "users won't understand this" with no evidence: reroute to ux-research's pile or drop).
- Discard evidence-tier inflation (assumed presented as verified — downgrade or drop).
- **Moderator fact-check:** before spending a refuter, ground-truth any claim you can verify in one cheap command (git state, a live count, a file's existence). A finding built on a stale snapshot dies here for free — log it as refuted-at-merge.
- Corroboration count strengthens confidence a defect EXISTS, not its severity — a 7-seat pile-on can still be a minor once refuted (seats share the same surface, so they co-observe the same shallow symptom).
- Sort: blockers/blocks first, then majors/conditions, then rest.

## Layer 6 — Adversarial verification (standard/deep; replaces all-vs-all debate)

For every surviving **blocker/block and major/condition**: spawn refuter agents in parallel (1 per finding at standard, 2 at deep — second refuter gets a different lens: "wrong on facts" vs. "true but immaterial to the decision").

Refuter prompt: "Try to kill this finding: `<finding>`. Evidence pack: `<pack>`. Attack the factual basis, the severity, or the materiality to the decision at stake. Return `REFUTED` (+why), `DOWNGRADED` (+new severity +why), or `STANDS` (+what you tried). You earn nothing by agreeing — a lazy STANDS is a failure; a wrong REFUTED kills a real issue. Be honest, not agreeable."

Four attack angles to bake into every refuter prompt (they killed or downgraded most findings in live runs):
- **"Already exists" check:** when the finding claims a capability is missing (no fallback, no drill-in, no dedup, no retry), instruct the refuter to hunt the source for it — seats reliably under-read the codebase and report existing mitigations as absent. The decisive read is often the BASE config/merge semantics a seat skipped (base settings a profile merges into, default branch of a policy) — send the refuter there by name.
- **Fix-sanity check:** attack the finding's proposed fix too — a fix that is reckless (e.g. planting fake writes against live data), economically wrong for the system's cost model, or already-implemented architecture discredits the severity even when the observation is real.
- **Planted-probe check:** when a seat cites a test artifact as evidence of a live defect (a fixture, stub, canary, quarantine entry, seeded failure), make the refuter check whether it is the test system's own deliberately planted probe — a passing mechanism-test misread as a production failure has killed findings in live runs.
- **Incident-attribution check:** when a finding credits a control with having contained a past incident ("the gate stopped X"), make the refuter verify WHICH mechanism actually did the containing — incident lore reliably migrates credit to the most visible control, while the real fix (a dedup key, an upstream validator, a notification batcher) sits elsewhere; mis-attributed credit inflates the cost of removing the visible control.

Give each refuter the specific source pointers to check when you know them — a refuter that must rediscover the codebase from scratch wastes its tokens on navigation instead of attack.

Apply verdicts: refuted findings drop (logged), downgraded findings re-rank. Exception: an unrecoverable-harm gatekeeper block (breach, legal violation, exclusion) survives refutation unless the refuter shows the *standard is misapplied* — severity taste doesn't kill those. Minors/ideas pass through unverified (cost > value).

At **deep**, add one completeness critic in parallel with refuters: "Given this decision, evidence pack, and these findings — what did every seat miss? Un-run angle, unexamined assumption, absent seat?" Its output feeds the verdict's gaps section (or a supplemental seat run if it names a missing seat with jurisdiction).

## Layer 7 — Conflict resolution (only where needed)

Find genuine conflicts among surviving findings (recommend opposite actions, or a fix for one violates another). Usually 0–3. For each, spawn ONE head-to-head agent: both findings + both seats' charter sections + evidence pack → "Resolve: adjudicate with the evidence, or synthesize a path satisfying both, or declare a true value conflict." Never all-vs-all — narrow debate is the only debate that earns its cost.

Precedence ladder for adjudication (and for your own final ranking):
1. **Unrecoverable-harm standards block** — stands unless standard misapplied; cheapest compliant path attached.
2. **Strong user/behavioral evidence** — beats every opinion. Moderate/anecdotal competes, doesn't auto-win.
3. **Economics that don't close (arithmetic shown)** — beats preference and craft.
4. **Feasibility cost** — reshapes scope of whatever survives 1–3.
5. **Taste/preference** — loses to all above; surviving taste conflicts go to the operator.

Evidence-free deadlock → resolution is the cheapest test that settles it (ux-research/analytics seats will have proposed one). True value conflict → don't fake a resolution; escalate to operator with both cases stated.

## Layer 8 — Verdict

```markdown
# DEBATE VERDICT — [target]
**Decision at stake:** [question] → **Answer:** [direct answer + one-line why]

## Confirmed blockers (survived adversarial verification)
- [finding] — [seat(s)] — [fix / exit criteria]
## Confirmed majors
- [finding] — [seat(s)] — [fix]
## Resolved conflicts
- [A vs B] — [resolution + precedence rule applied]
## Tests before commitment
- [assumption] — [cheapest test + cost]
## Escalations (operator must decide)
- [conflict] — [case A vs case B, one line each]
## Minors & ideas (unverified)
- [compact list]
## Coverage & integrity
- Seats run: [list] · Excluded: [list + why]
- Refutation: X findings verified → Y stood, Z refuted, W downgraded
- Gaps: [completeness-critic output / evidence that couldn't be gathered]

## Action list (ordered by impact per unit effort)
1. [action] — [source finding]
```

Direct answer required — never a mushy "do everything" list. A finding refuted is reported as refuted; don't resurrect it softened.

## Rules

- Moderator holds no opinion. Procedure, lanes, evidence tiers, precedence — nothing else.
- Evidence pack BEFORE spawning, identical for all seats; enforcement of "no vibes findings" happens at merge.
- Round-1 blindness is sacred — never leak one seat's output into another's prompt.
- Refuters must genuinely attack, not perform review-theater; instruct against both sycophancy and lazy agreement.
- This skill critiques and decides; it does not implement. Offer fixes as a separate opted-in follow-up.
- Charter files are the single source of seat expertise — update them there, not in this file.

## Self-improvement (standing rule)

This skill is self-improving. After every debate (verdict delivered, or aborted), spend one minute on friction: a seat that manufactured findings for lack of jurisdiction (roster-trigger gap), an evidence-pack omission the refuters had to correct, a merge-time fact-check class worth naming, a refuter attack angle that killed findings and deserves baking into the prompt template, a verdict-template line that misled a reader. If the lesson generalizes, fix it in the same session — pipeline/mechanics here in SKILL.md, seat expertise in `charters/` (never seat content in this file). Rules:

- **Process-agnostic only.** This skill runs across many projects: never bake in project names, paths, product context, or one case's verdict. Case-specific lessons belong in that project's notes; only reusable *method* lands here.
- Fix the doc, don't grow it: prefer correcting the wrong line over appending a caveat; prune superseded text. Date an observation only when the date disambiguates.
- Keep the mechanism ranking intact — a fix that strengthens evidence grounding outranks one that adds topology (more seats/refuters is rarely the answer).
