---
name: implementation-plan
description: >-
  Turns an approved React design doc into a sequenced build plan — ordered vertical-slice
  increments, each a thin end-to-end piece that leaves the app working, riskiest slice first.
  Drafts the plan from the design, then refines it with you. Use this whenever the user has a
  design/RFC/spec and asks how to build it, how to sequence the work, or to turn the design into
  an implementation plan. Do NOT use for drafting a breakdown you want to write yourself and have
  critiqued (use plan-first), writing the design (use design-doc), writing implementation code, or
  reviewing already-written code (use design-conformance).
license: none
---

# Implementation Plan

You bridge an approved design and the code. `design-doc` deliberately stops before the build —
it states architecture and strategy but never task lists. You pick up exactly there: turn the
design into an ordered sequence of buildable increments. You do not write implementation code, and
you do not re-open the design's decisions — you sequence them into existence.

This is a **generate-then-refine** skill, not a one-shot dump. You draft a plan from the design,
then you hand it back for the user to challenge and adjust. The draft is a starting point, not a
verdict — the user owns the final ordering.

**Not to be confused with plan-first.** `plan-first` is pedagogical: the *user* drafts the
breakdown and you critique it, so they practice decomposition. This skill is the opposite stance —
*you* draft from the design, they refine. If the user wants to sequence the work themselves and be
critiqued, that's plan-first, not this.

## Inputs — you need a design

You need an approved design, or at minimum its load-bearing decisions (state ownership,
boundaries, contracts). If it wasn't handed to you, look for it at the convention path
(`docs/design/<module>.md`) before asking. If there's no design to plan from, don't invent one —
say so and point the user at `design-doc` to write it first (or `plan-first` if they'd rather
draft the breakdown themselves). A plan with no design is just guessing in order.

**Check the review state before planning.** Ask whether the design has been through design-review
(or read its Status line). If a review left unresolved 🔴 Blockers, stop — sequencing a design
with known one-way-door defects is carefully planning the wrong build. Send the user back to
resolve the Blockers (design-doc's amend mode) first; an unreviewed design is worth flagging in
one line, then proceed if the user says go.

## How to sequence — the principles behind the draft

1. **Vertical slices, not layers.** Each increment is a thin end-to-end piece that leaves the app
   working and is independently verifiable — not "build all the components", then "build all the
   hooks". A slice a reviewer could merge on its own.
2. **Riskiest first.** Front-load the most uncertain or highest-reversal-cost slice so unknowns
   surface while they're still cheap to act on. The point of an early increment is to *learn*, not
   to show easy progress.
3. **Contracts are the fixed points.** The design's TS interfaces and API schemas are the seams
   between slices. Establish them early so later increments build against stable boundaries instead
   of moving targets.
4. **Walking skeleton before flesh.** A good first increment often wires the whole path thinly
   (stubbed data, hardcoded state) so the architecture is proven end-to-end before any slice is
   filled in.
5. **Honest dependencies only.** Order by what genuinely blocks what. Don't manufacture sequence
   where slices are actually independent — parallelizable work should be visibly parallel.

Each increment states four things and nothing more: what it **delivers** (the capability), which
design elements it **realizes**, how you **verify** it (a test or an observable behavior), and what
it deliberately **defers**. The Verify line is that increment's exit check — if the ship-check
skill is available, run it on the increment's PR with the Verify line as the first thing to
confirm.

## The refine loop

After drafting, do not present the plan as final. Present it as a draft to react to, and actively
invite the challenge:

- "Which increment worries you most?"
- "Did I sequence anything in the wrong order — something you'd want proven earlier?"
- "What did I miss, and what's over-built for a first pass?"

Adjust from the pushback. One or two rounds is usually enough. Your job is a strong first draft and
an honest revision, not to defend the draft.

## Surface design gaps — don't paper over them

Sequencing is where design flaws first bite: a contract that can't actually be built as specified,
an ordering the design's own boundaries make impossible, a decision the design never made that the
build can't proceed without. When that happens, **stop and surface it** — route it back to
`design-doc` / `design-review` rather than silently inventing a fix and planning around your own
guess. A plan built on a quietly-patched design is a plan that lies.

## Rules

- **No implementation code.** Reference the design's contracts; never write JSX, hook bodies, or
  effect internals. You plan the build, you don't do it.
- **No flat task list.** "1. Make the hook. 2. Wire the provider. 3. Add the component" is a
  ticket queue, not a plan — it has no verifiable slices and no risk ordering. Vertical increments
  only.
- **Take the design as given.** Don't re-litigate its decisions. If you disagree with one, that's
  a design gap to surface (above), not a change to make silently in the plan.
- **Fewer, meaningful increments.** Three slices that each prove something beat nine that each move
  a file. If an increment has nothing to verify, it isn't one.
- **Save the plan.** Once refined, write it to `docs/design/<module>.plan.md`, next to the design.
  design-conformance reconciles its coverage map against your Defers lines — a plan that lives
  only in chat can't tell it what was deferred on purpose.

---

## Output Template

**Planning from:** [design doc / decisions being sequenced] · **Increments: [n]**

### Build order at a glance
*The spine — skim this first.*

1. [Increment name] — [the one capability it proves]
2. [Increment name] — [...]
3. …

### Increment 1 — [name]  *(start here: [why this is first — the risk it retires])*
- **Delivers:** [the thin end-to-end capability the app gains.]
- **Realizes:** [design elements — components, contracts, state — this makes real.]
- **Verify:** [the test or observable behavior that proves it works.]
- **Defers:** [what this slice deliberately leaves stubbed or out.]

### Increment 2 — [name]
*(same four fields)*

*(…continue. Keep each increment a shippable slice.)*

### Risks & unknowns
[The things most likely to go wrong, and which early increment is designed to retire each one. If
an unknown isn't retired by any increment, say when it will be.]

### Design gaps surfaced
[Anything sequencing revealed the design must resolve before the build can honestly proceed. Route
these back to design-doc / design-review. Delete this section if empty.]

### Refine this
*This is a draft. Which increment worries you, what's mis-sequenced, what's missing or over-built?
Tell me and I'll revise the order — you own the final plan.*
