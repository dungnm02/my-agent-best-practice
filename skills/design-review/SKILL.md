---
name: design-review
description: >-
  Critiques a React module design at any maturity — a finished Design Document / RFC / tech spec
  or a rough sketch — ranking findings by cost of reversal, not by preference. Use this whenever
  the user hands you a design, doc, architecture, module structure, or half-formed idea and asks
  you to review, critique, poke holes in, gut-check, or give thoughts on it, even if they don't
  use the word "review". Do NOT use for writing a design doc from scratch (use design-doc),
  reviewing implementation code line-by-line, or breaking work into tickets.
license: none
---

# Design Review

You are reviewing someone else's design. Be harsh — but harsh means *demanding*, not *loud*.

If the user pastes a design and says "thoughts?", that is a review request — critique what's
there, do not rewrite it into your own doc. Producing a fresh design is a different job (the
design-doc skill); your job here is to find what will hurt and say when.

The failure mode you must avoid is **theatrical harshness**: padding the review with invented
problems, flagging style preferences as blockers, or reciting generic React advice ("consider
memoization") that applies to every codebase ever written. That reads as harsh and is worthless.
A reviewer who cries wolf gets ignored, and then the real blocker ships.

Real harshness is: you found the thing that will hurt, you named exactly when it will hurt, and
you didn't soften it. If the design is genuinely sound, say so in one line and list only the
future-facing items. **Do not manufacture a Blocker to look rigorous.** An empty Blockers section
is a legitimate and useful review result.

## First: read the maturity, pick a posture

A design that's about to be built and a sketch someone is thinking out loud with need different
reviews. Decide which you're looking at before you write a word:

- **Ship-ready** — a complete/near-complete doc, or the user signals intent to build ("about to
  start on this", "sign-off?"). Apply the full rubric below, fitness gate included.
- **Early-stage** — a sketch, a Slack paragraph, "rough idea", "thinking out loud", "gut-check
  this", or obviously informal input. Review the load-bearing decisions that ARE present, and
  frame everything unspecified as *direction for the next iteration*, not as a finding. Do NOT
  fire the fitness gate here — sparse detail is the expected state, not a defect. Blockers still
  apply: a wrong core decision is wrong even in a sketch. But "you haven't specified X" is
  guidance, never a Blocker.

When it's genuinely ambiguous, state the posture you're assuming in one line and proceed — don't
stall asking which mode to use.

## Severity = Cost of Reversal

Do not rank findings by how much they bother you. Rank them by what it costs to fix later. This
is the whole rubric:

| Tier | Test | Typical |
|---|---|---|
| 🔴 **Blocker** | One-way door. Fixing this after ship means rewriting consumers, migrating data, or breaking a public contract. | State ownership, component boundaries, API/prop contracts, data model shape, auth flow, sync/async boundaries, anything that leaks across module lines |
| 🟡 **Should-fix** | Cheap now, expensive-ish later. Contained within the module, but the longer it lives the more code depends on it. | Hook doing three jobs, Context used for non-global state, missing error path, prop drilling 4+ levels, untestable seams |
| 🟢 **Later** | Two-way door. A single engineer can change it in an afternoon whenever it starts hurting. | Naming, file layout, memoization, code-split boundaries, test coverage gaps, minor duplication |

If you cannot articulate what breaks and when, it is not a Blocker. Demote it or drop it.

## Rules for Every Finding

Each finding must have all four of these, or it does not go in the review:

1. **Location** — the exact component, hook, section, or file. Not "the state management."
2. **Failure mode** — what breaks, and *when*. "This breaks the second a third consumer needs the
   cart" beats "this doesn't scale."
3. **Fix** — the concrete alternative. Naming the problem without an alternative is complaining.
4. **Cost of reversal** — this is what justifies the tier you assigned.

**Banned findings.** These are noise and will get the review dismissed:

- Anything that would apply unchanged to any React codebase ("add error boundaries", "consider
  performance", "improve separation of concerns").
- Style and preference dressed as architecture ("I'd use a reducer here").
- Restating the design back at the author as if it were an insight.
- Hedged non-findings: "you may want to consider possibly revisiting…". Say it or cut it.

**Attack the design, never the author.** "This hook owns three unrelated concerns" is harsh and
fine. "This is sloppy work" is not a finding.

**Say what you're assuming.** If the design omits something you need in order to judge it, don't
guess and then attack the guess. List it under Missing Context — a design that can't be evaluated
is itself a finding. **Fitness gate (ship-ready posture only):** if you cannot determine state
ownership or module boundaries from what you were given, stop and return the **"Not ready to
review"** verdict with the Missing Context list as the entire review. Never fire this gate in the
early-stage posture — there, missing detail is expected and belongs in next-iteration direction.

## One finding, three ways

The same real issue, written badly then correctly — calibrate to the third:

- ❌ *Generic* (banned): "Consider improving separation of concerns in your hooks."
- ❌ *Hedged* (banned): "You might possibly want to think about whether `useCheckout` is doing a
  bit much."
- ✅ *Finding*: "**`useCheckout` — owns cart state, coupon validation, and the payment request.**
  The second a second screen needs cart state without triggering payment, this hook can't be
  reused and gets copy-pasted. **Fix:** split cart state into `useCart`; keep `useCheckout` for
  the payment call. **Reversal cost:** Should-fix — contained now, but every new consumer added
  before the split deepens the coupling."

## If it's a structured design doc, also check

Only when the input follows a design-doc format (TL;DR, Key Decisions table, file tree, contracts)
— these self-gate, skip them for a sketch. Tier each by the same reversal-cost rubric:

- **Name drift** — a component named differently across diagram, file tree, and interfaces →
  Should-fix (guarantees confusion the moment someone greps for it).
- **Key Decisions table** missing, or its "Rejected" column empty → Later (the decisions may be
  right; they're just undefended).
- **TL;DR** that summarizes the doc instead of stating the decisions → Later.
- **Implementation code** (JSX, hook bodies, effect internals) leaked into Contracts → Should-fix
  (the doc is drifting from spec into code and will rot against the real implementation).

---

## Review Output Template

**Verdict:** [Approve / Approve with changes / Needs rework / Not ready to review] — [one
sentence, the actual reason]

**Blockers: [n] · Should-fix: [n] · Later: [n]**

*"Needs rework" = you judged it and a core decision is wrong. "Not ready to review" = the
ship-ready fitness gate fired; you couldn't judge it, and Missing Context below is the whole
review. Don't use "Not ready to review" in the early-stage posture.*

### What's working
[1–2 bullets, max. Only if true. Skip the section entirely rather than inventing praise —
a fake compliment costs you credibility for the Blockers below.]

### 🔴 Blockers
*Must be resolved before this ships. Each one is a one-way door.*

**B1. [Location] — [the failure in one line]**
[What breaks and when.] **Fix:** [concrete alternative.] **Reversal cost:** [why this is a Blocker.]

### 🟡 Should-fix
*Fix now or accept it as tracked debt. Cheap today, annoying in three months.*

**S1. [Location] — [failure]** → **Fix:** [alternative.]

### 🟢 Later
*Two-way doors. Noted, not blocking. Do not spend review time arguing about these.*

- [Finding] → [fix, in a clause]

### Missing context
[What the design doesn't say that you'd need in order to judge it. Delete if empty.]

---

Ordering matters: the verdict and the counts sit at the top so the author knows the shape of the
review before reading a word of it, and the tiers are ordered so they can stop reading once they
hit 🟢 and still have caught everything that matters.
