# Usage — loading the skills and triggering the loops

Two halves to using this repo: getting the skills *loaded* into your agent,
and saying the *phrases* that fire each one. Skills trigger off their
`description` frontmatter — every phrase quoted below is drawn from the
descriptions, so it works for natural triggering, not just explicit calls.

## 1. Make the skills visible to your agent

This repo is the source of truth, but agents only read their own skill
directories. Point yours at the folders you want:

- **Claude Code, per-project:** copy or symlink skill folders into the
  project's `.claude/skills/`:

  ```sh
  ln -s ~/my-agent-best-practice/skills/clarify-task .claude/skills/clarify-task
  ```

- **Claude Code, everywhere:** put them in `~/.claude/skills/` instead —
  every project gets the loops.
- **Other agents / Claude apps / API:** same folders, dropped wherever that
  tool's Skill spec looks. The skills contain no agent-specific assumptions,
  so they work as-is.

Verify with `/skills` (Claude Code) or just ask: "what skills do you have
available?"

## 2. Two ways to trigger any skill

- **Explicit** — name it: `/clarify-task`, or "use the design-review skill
  on this." Always works; use it when you want a specific stage.
- **Natural** — say something matching the description and the agent loads
  it on its own. That's what the trigger-rich descriptions are for.

## 3. The small loop, phrase by phrase

For bug fixes, minor changes, and small tickets.

| Stage | Skill | Say something like |
|---|---|---|
| Requirement | `clarify-task` | "I picked up this bug ticket: … help me get started" · "users report X is broken" · "can you change how Y works?" |
| Plan | `plan-first` | "help me break this down" · "plan this ticket with me" |
| Build | — | (just work; no skill owns this stage) |
| Review | `ship-check` | "check my PR before I open it" · "is this ready to ship?" |

You only have to *start* the loop. `clarify-task`'s Task Brief ends with a
**Route** line that names what's next — "trivial, just fix it", "small loop,
plan-first next", or "escalate to the design loop". The escalation check is
automatic: you don't need to know your fix touches a contract; the skill
checks `docs/design/` and the 🔴-tier decisions for you.

## 4. The design loop, phrase by phrase

For new modules and refactors that move state ownership, contracts, or
boundaries.

| Stage | Skill | Say something like |
|---|---|---|
| Author | `design-doc` | "write a design doc for the checkout module" · "I need an RFC for refactoring X" |
| Review | `design-review` | "review this design" · "poke holes in this" · pasting a sketch + "thoughts?" |
| Amend | `design-doc` (amend mode) | "apply the review findings to the doc" |
| Plan | `implementation-plan` | "turn the design into a build plan" · "how should I sequence this?" |
| Build | — + `ship-check` | each increment's PR: "ship-check this — the Verify line is [X]" |
| Verify | `design-conformance` | "check the implementation against the design" · "does the code match the doc?" |

## 5. Loops survive across sessions

You don't need one long conversation. The design skills write durable
artifacts at fixed paths — `docs/design/<module>.md` and
`docs/design/<module>.plan.md` — and every downstream skill looks there
*before asking you for anything*. Next week, in a fresh session, "check the
checkout code against its design" just works: `design-conformance` finds the
doc and the plan on its own.

The doc's **Status** line (Draft / Approved / Superseded) is how the skills
know where in the loop you are: `design-review`'s approval flips it to
Approved, amending an Approved doc returns it to Draft, and
`implementation-plan` will balk at planning an unreviewed or Blocker-laden
design.

## 6. The learning skills ride along

The loops occasionally hand off to the knowledge skills when they're
available — a review finding you never saw coming goes to `learning-log`, a
concept you hit mid-build goes to `concept-deep-dive`. Those hand-offs are
always optional offers, never requirements: every skill works standalone.
