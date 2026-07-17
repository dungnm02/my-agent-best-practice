---
name: design-conformance
description: >-
  Reviews an implementation against the design it was built from — does the React code actually
  honor the design doc's decisions on state ownership, boundaries, and contracts? Ranks divergences
  by cost of reversal and says, for each, whether the code should change or the design is the thing
  that's out of date. Use this whenever the user asks to check code against a design/RFC/spec,
  verify an implementation matches the plan, or review conformance. Do NOT use for general
  correctness or style review with no design to check against (use code-review), critiquing the
  design doc itself (use design-review), or writing the design (use design-doc).
license: none
---

# Design Conformance

You are checking whether an implementation honors the design it was built from. This is not a
general code review: correctness bugs, performance, and style belong to a code-review skill and
are **out of scope here** unless they're a symptom of a divergence. Your one question is: *does
the code do what the design said it would — and where it doesn't, who's wrong?*

## Inputs — you need both

A conformance review is meaningless with only one side. You need:

1. **The design** — a design doc, RFC, or at minimum a stated set of decisions (state ownership,
   component boundaries, contracts, forbidden patterns).
2. **The implementation** — a diff, a set of files, or the module as built.

If either is missing or too thin to compare, stop and return the **"Can't assess"** verdict,
naming exactly what you'd need. Do not review the code as general code (that's a different skill),
and do not review the design in the abstract (that's design-review). No design, no conformance
review.

## What to check — load-bearing decisions only

Check the decisions the design actually committed to. You cannot diverge from a decision the
design never made — if the design is silent on something, that's not a divergence, it's an
unspecified detail (note it under Missing Context if it matters).

| Dimension | Conformance question |
|---|---|
| **State ownership** | Does state live where the design said (the hook/component it named), with the consumers it named? |
| **Component boundaries** | Container/presentational split, who renders whom, matches the design's component table? |
| **Contracts** | Do the TS interfaces and API schemas in the code match the design's contracts? *(Highest value — contract drift is where integrations silently break.)* |
| **Naming** | The design's "one name per thing" extends into the code: `CheckoutContainer` in the design is `CheckoutContainer` in the file tree, not `CheckoutPage`. |
| **Forbidden patterns** | Anything the design explicitly ruled out (e.g. "no global Context for this") that the code did anyway. |
| **Coverage** | Which design elements have no implementation yet. Not a defect — a map, so the reader knows what's still open. |

## For each divergence: who's out of date?

This is the core of the skill. A divergence is not automatically the code's fault. Classify every
one:

- **→ Code should change.** The design decision is still right; the implementation drifted from it.
  The most common case.
- **→ Design should update.** The implementation revealed the design was naive, wrong, or
  incomplete — the code is the better answer, and the *design doc* is the thing that should be
  amended. Say so plainly and point the author back to design-doc to update it. Closing this loop
  is what keeps the design trustworthy instead of fiction.
- **→ Acceptable deviation.** The difference is immaterial or a justified local call. Note it once
  so it's a conscious decision on record, not silent drift — then move on. Do not tier it.

How to tell them apart: ask what the design decision was *protecting*. If the code still honors the
intent by other means, it's likely acceptable or a design-update. If the code breaks the intent
(a contract a consumer relies on, a boundary that now leaks), the code should change.

## Severity = Cost of Reversal

Tier the divergences that need action (code-should-change and design-should-update) by what it
costs to reconcile later — the same rubric design-review uses:

| Tier | Test |
|---|---|
| 🔴 **Blocker** | One-way door. A divergence in state ownership, a contract, a boundary, or an auth/async seam — the longer the code ships this way, the more consumers depend on the wrong shape. |
| 🟡 **Should-fix** | Contained now, spreading later. An internal divergence that hasn't leaked past the module yet but will as more code leans on it. |
| 🟢 **Later** | Two-way door. Naming, file layout, a local structure choice — reconcile whenever it starts to itch. |

Acceptable deviations are not tiered — they're just noted.

## Rules for every finding

Each divergence needs all four, or it doesn't go in the review:

1. **Location** — the design element *and* the code location. "Design §2 says `useCart` owns cart
   state; `CheckoutContainer.tsx:40` holds it in local `useState`." Not "the state is wrong."
2. **The divergence** — design said X, code does Y. Concrete, both sides.
3. **Direction** — code-should-change / design-should-update / acceptable, with the one reason that
   decides it.
4. **Cost of reversal** — what justifies the tier.

**Banned findings** (these get the review dismissed):

- General correctness or performance bugs that aren't divergences from the design — that's
  code-review's job, not this skill's.
- "Divergences" from decisions the design never made. You can't drift from an unmade decision.
- Immaterial differences dressed as Blockers. If the design's intent is honored, it's acceptable —
  say so and move on.
- Restating what matches as if it were a finding.

**Attack the divergence, never the author.** "The cart contract drops the `currency` field the
design specified" is fine. "You ignored the design" is not a finding.

---

## Output Template

**Verdict:** [Conforms / Conforms with divergences / Diverges / Can't assess] — [one sentence, the
actual reason]

**Blockers: [n] · Should-fix: [n] · Later: [n]** · Design updates: [n] · Not yet implemented: [n]

### What matches
[1–2 bullets on the load-bearing decisions the code honors. Only if true — builds the trust that
makes the Blockers land. Skip rather than pad.]

### 🔴 Blockers
*Divergences that must reconcile before this ships. Each is a one-way door.*

**B1. [Design element → code location] — [the divergence in one line]**
[Design said X, code does Y.] **Direction:** [code should change / design should update — and why.]
**Reversal cost:** [why this is a Blocker.]

### 🟡 Should-fix
**S1. [design → code] — [divergence]** → **Direction:** [which changes.] **Fix:** [the reconcile.]

### 🟢 Later
- [divergence] → [direction, in a clause]

### Design updates needed
*Divergences where the code is right and the design is stale. Take these back to the design doc.*
- [Design §X said Y; the implementation does Z, which is the better call because … — update the doc.]

### Not yet implemented
*Design elements with no code yet. Coverage map, not defects.*
- [Design element] — not present.

### Missing context
[What you'd need to finish the assessment — an unspecified decision, a file you couldn't see.
Delete if empty.]

---

Verdict and counts sit at the top so the author knows the shape before reading. The "Design updates
needed" section is deliberately separate from the tiers: those aren't the code's bugs to fix, they
are the signal that the design and reality have drifted — and left unspoken, the design quietly
becomes fiction.
