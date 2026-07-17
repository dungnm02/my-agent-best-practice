# Skills

This directory holds reusable **Agent Skills** — self-contained folders that
package instructions (and optionally scripts/references/assets) an AI agent
can load on demand. The format is agent-agnostic: any tool that implements
the open Skill spec (Claude Code, Claude Desktop/apps, the Claude API skills
feature, etc.) can consume a skill folder from here as-is, no adaptation
needed.

## Layout

```
skills/
  <skill-name>/
    SKILL.md          # required
    scripts/           # optional — executable helpers
    references/         # optional — docs loaded on demand
    assets/              # optional — templates/files used in output
```

Each skill is a directory named after the skill (lowercase, hyphenated).
Everything the agent needs lives inside that one directory.

## Available skills

**Knowledge:**

- [`challenge-me`](challenge-me/SKILL.md) — interrogates you like a skeptical
  senior engineer before giving answers, to build independent engineering
  judgment instead of AI reliance.
- [`learning-log`](learning-log/SKILL.md) — keeps a durable journal of TILs
  and reasoning gaps, and quizzes you on past entries so they stick.
- [`code-tour`](code-tour/SKILL.md) — guided tour of unfamiliar code (map,
  one flow end-to-end, invariants), ending with you tracing a path yourself.
- [`concept-deep-dive`](concept-deep-dive/SKILL.md) — teaches a concept from
  first principles when real work touches it, with an exercise tied to your
  actual code.

**Productivity:**

- [`clarify-task`](clarify-task/SKILL.md) — interrogates the requirement
  before work starts on a bug fix or small change (problem, repro, scope,
  done criteria), then routes it: quick fix, small loop, or escalate to the
  design loop.
- [`plan-first`](plan-first/SKILL.md) — you draft the ticket breakdown
  (steps, unknowns, risks, estimate); the agent critiques it like a tech
  lead before any code is written.
- [`ship-check`](ship-check/SKILL.md) — pre-PR ritual: you self-review your
  diff and predict reviewer feedback before the agent reveals what it found.
- [`standup-notes`](standup-notes/SKILL.md) — turns git history and session
  context into crisp Yesterday / Today / Blockers standup notes.

**Design loop (author → review → plan → verify):**

- [`design-doc`](design-doc/SKILL.md) — generates skimmable Design Documents
  (RFC / tech spec) for React modules: decisions-first template, word budgets,
  and coverage-based diagram rules.
- [`design-review`](design-review/SKILL.md) — critiques an existing React
  design/RFC, ranking findings by cost of reversal (Blocker / Should-fix /
  Later) with a banned-findings list that filters out generic noise.
- [`implementation-plan`](implementation-plan/SKILL.md) — turns an approved
  design into a sequenced build plan of vertical-slice increments (riskiest
  first), drafted from the design then refined with you.
- [`design-conformance`](design-conformance/SKILL.md) — reviews an
  implementation against the design it was built from, ranking divergences by
  cost of reversal and saying, for each, whether the code or the design is the
  thing that's out of date.

## Which loop?

Two loops, sized to the work. Route by **cost of reversal**, not by effort:

```
Small loop:  clarify-task → plan-first → build → ship-check
                  │
                  │ escalate if the change touches a 🔴-tier decision
                  │ (state ownership, contracts, boundaries) or
                  │ contradicts a doc in docs/design/
                  ▼
Design loop: design-doc → design-review → implementation-plan
                        → build (+ ship-check) → design-conformance
```

- **Trivial fix** (typo-level, no behavior change): skip the loop; at most a
  2-minute ship-check.
- **Bug fix / minor change**: small loop. `clarify-task` pins down the
  requirement and confirms nothing 🔴-tier moves; `plan-first` breaks it
  down; `ship-check` gates the PR.
- **New module or refactor that moves state ownership, contracts, or
  boundaries**: design loop — even if the diff looks small. A quick fix that
  rewrites a contract is a design change wearing a small-loop costume.

## Creating a new skill

1. Copy `skills/TEMPLATE/` to `skills/<your-skill-name>/`.
2. Fill in the frontmatter in `SKILL.md`:
   - `name` — must match the directory name, lowercase-hyphenated, ≤ 64 chars.
   - `description` — third person, states **what** the skill does and
     **when** to use it. This is the only part of the skill an agent sees
     before deciding to load it in full, so front-load the trigger
     keywords/situations.
3. Write the instructions in the body. Keep `SKILL.md` itself short — once
   triggered, the whole file loads into context. Move long or
   rarely-needed material into `references/` and link to it instead of
   inlining it.
4. Delete whichever of `scripts/`, `references/`, `assets/` you don't use.

## Principles

- **Progressive disclosure**: only the `name` + `description` need to be
  cheap to scan up front; the body and any `references/` files are loaded
  only when the skill actually triggers.
- **One skill, one job**: keep scope narrow so the `description` can be
  precise and agents don't misfire on unrelated tasks.
- **Prefer scripts over prose for deterministic steps**: if a step is "run
  this exact command/transform", put it in `scripts/` rather than asking
  the agent to reproduce it from instructions each time.
- **No agent-specific lock-in**: don't assume a particular tool's file
  layout (e.g. `.claude/`) inside a skill's own instructions — the same
  folder should work wherever the Skill spec is supported.
