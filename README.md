# Claude Debate Skills

Two [Claude Code](https://claude.com/claude-code) agent skills that run a structured, adversarial multi-agent critique of a decision and return a single defensible verdict.

Both are **layered critique machines**, not chat panels. The model acts as a moderator with no substantive opinion; quality comes from four mechanisms, in this order:

1. **Evidence grounding** — an agent critiquing a rendered page (or a dated price) beats five critiquing a description.
2. **Independent blind critique** — expert seats run in parallel and never see each other's output.
3. **Adversarial verification** — every blocker and major gets a refuter agent whose job is to kill it.
4. **Precedence-based synthesis** — conflicts resolve on a fixed ladder, not on vibes.

Debate is used as a scalpel, only where verified findings genuinely conflict — never as an all-vs-all ritual.

## The two skills

| Skill | Use for | Seats |
|---|---|---|
| `software-debate` | Software products, features, specs, design choices, repos, launches | 14 fixed expert seats across 4 charters: builders, gatekeepers, truth sources, market reality |
| `general-debate` | Everything else — purchases, hardware builds, cooking, travel, plans, business and life decisions | 7 domain-agnostic archetypes instantiated into the topic, plus up to 2 custom specialist seats |

`general-debate` adds a **prime directive: research, don't guess.** Prices, specs, availability, versions, and rules are volatile facts — they must be looked up with a source and an access date, never recalled from model memory. Anything that can't be verified is tagged `assumed` and surfaces in the verdict as a pre-commitment test.

## Install

Copy either or both directories into your skills folder:

```bash
git clone https://github.com/msaza2/claude-debate-skills.git
cp -r claude-debate-skills/software-debate ~/.claude/skills/
cp -r claude-debate-skills/general-debate ~/.claude/skills/
```

Restart Claude Code (or start a new session) so the skills are picked up.

## Use

Both skills are **explicit-invocation only** — they never fire proactively, because a full pipeline spawns a lot of agents.

```
/software-debate is this checkout redesign launch-ready?
```

```
/general-debate which of these three laptops for a 4-year dev machine, $1500 cap
```

You'll be asked for the target, the decision at stake, a depth, and a context pack:

- **`quick`** — seats run, no refuter layer; verdict from merged findings (and it says so).
- **`standard`** — full pipeline, 1 refuter per blocker/major. Default.
- **`deep`** — full pipeline, 2 refuters per finding with different lenses, plus a completeness critic.

## What you get back

A verdict with a direct answer — never a mushy "consider everything" list — plus confirmed blockers, confirmed majors, resolved conflicts with the precedence rule applied, tests to run before committing, escalations that are genuinely yours to decide, and a coverage section stating which seats ran, which were excluded and why, and how the refutation pass went (X verified → Y stood, Z refuted, W downgraded).

A finding that got refuted is reported as refuted. It doesn't come back softened.

## Structure

```
software-debate/
  SKILL.md              # the 8-layer pipeline; moderator mechanics only
  charters/
    builders.md         # product-manager, designer, engineer, sre
    gatekeepers.md      # qa, security, legal-privacy, accessibility
    truth-sources.md    # ux-research, analytics, support-cs
    market-reality.md   # sales, marketing, finance
general-debate/
  SKILL.md              # pipeline + volatility triage + research rules
  charters/
    archetypes.md       # craftsman, fit-advocate, value-analyst, risk-analyst,
                        # evidence-skeptic, logistics, alternatives-scout
```

Seat expertise lives in the charters; pipeline mechanics live in `SKILL.md`. Keep it that way when editing — both skills carry a standing self-improvement rule that says so, and it's what keeps them process-agnostic across projects.

## License

MIT
