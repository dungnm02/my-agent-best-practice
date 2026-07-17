---
name: clarify-task
description: >-
  Interrogates the requirement before any work starts on a bug fix, minor change, or small
  ticket — pins down the actual problem, the repro, scope boundaries, and what "done" looks
  like, then routes the task to the right loop (quick fix, small loop, or full design loop).
  Use this when the user brings an issue, bug report, small ticket, or vague request ("fix
  this", "it's broken", "can you change X") and is about to start work. Do NOT use for
  designing a new module (use design-doc), challenging the user's own technical decisions
  (use challenge-me), or drafting the work breakdown (use plan-first — this skill ends where
  that one begins).
license: none
---

# Clarify Task

## Purpose

On small changes, the expensive failure isn't bad architecture — it's fixing the wrong
thing: patching the symptom instead of the cause, guessing at unstated acceptance criteria,
or discovering mid-fix that the "minor change" moves a load-bearing decision. This skill
interrogates the *requirement* until the task is unambiguous, then routes it. It is the
front door for small work, the way design-doc is the front door for modules.

Unlike challenge-me, the target here is the task, not the user — you're not testing their
knowledge, you're jointly pinning down what's actually being asked. Questions they can't
answer are findings, not failures: they name exactly what to go find out before coding.

## What to pin down

Ask **one question at a time**, starting from the biggest unknown. Stop as soon as the
brief below can be filled honestly — typically 3–6 questions, fewer for a well-written
ticket. Never run the full list against a task that's already clear; interrogation that
re-asks what the ticket states is noise.

| Dimension | The question behind it |
|---|---|
| **Problem vs symptom** | What's the expected behavior, what happens instead — and is the reported thing the disease or a symptom of something upstream? |
| **Repro** | Can it be reproduced on demand? Steps, environment, and the case where it *doesn't* happen (often the sharpest clue). |
| **Scope** | Which behavior/files are in; what's explicitly out. "While you're in there…" creep gets named and parked, not absorbed. |
| **Done criteria** | The observable check that closes the task — a passing test, a behavior demonstrable in the UI, an error gone from the logs. "Feels fixed" is not done. |
| **Constraints** | Anything that narrows the solution space: backwards compatibility, "don't touch X", a deadline, a consumer relying on current behavior (even the buggy one). |
| **Regression risk** | What shares code with the thing being changed — where would you look first if this fix broke something else? |

## Routing — the point of the interrogation

Small changes can be big-loop changes in disguise. Before writing the brief, check whether
the change touches a **🔴-tier decision** (the same cost-of-reversal vocabulary the design
skills use): state ownership, a contract/interface other code consumes, or a module
boundary. Also check `docs/design/` for a design doc covering the touched module.

- **Trivial** — typo-level, no behavior change worth planning. Skip straight to the fix;
  even ship-check runs in its 2-minute form.
- **Small loop** — real but contained: behavior changes, but no 🔴-tier decision moves.
  Route to plan-first for the breakdown, ship-check before the PR.
- **Escalate to the design loop** — the change moves a 🔴-tier decision, or contradicts an
  Approved doc in `docs/design/`. A "quick fix" that quietly rewrites a contract is how
  designs become fiction. Route to design-doc's amend mode (and design-review) instead of
  proceeding — or, if the code is right and the doc is stale, that's design-conformance's
  "design should update" call, not a silent drift.

When a fix contradicts a design doc, say so explicitly — never let the user discover the
conflict from a failed conformance review later.

## Output — the Task Brief

**Task:** [one line, the user's words tightened]
**Problem:** [expected vs actual, and whether it's cause or symptom]
**Repro:** [steps / environment — or "not yet reproducible: first step is X"]
**In scope:** [what changes] · **Out of scope:** [what explicitly doesn't]
**Done when:** [the observable check]
**Constraints:** [delete if none]
**Regression risk:** [what to re-test]
**Route:** [Trivial — just fix it / Small loop — plan-first next / Escalate — touches
[decision], go through design-doc amend + design-review]

## Rules

- **One question at a time.** A questionnaire dump gets skimmed; a single sharp question
  gets answered.
- **Proportionality.** A clear ticket needs a 30-second confirmation of the brief, not an
  interrogation. Match depth to ambiguity, not to ritual.
- **Unknowns are routable.** "We don't know the repro yet" doesn't block the brief — it
  becomes the first step in it.
- **Park scope creep, don't argue it.** Adjacent work spotted during questioning goes in
  one line under Out of scope; whether to do it later is the user's call.
- **Hand off, don't continue.** This skill ends at the brief. The breakdown belongs to
  plan-first, the pre-PR review to ship-check — don't absorb their jobs.

If the learning-log skill is available and the interrogation overturned the user's initial
framing (the reported symptom wasn't the problem, the "obvious fix" was out of scope),
offer to log it — misread requirements are among the most repeatable mistakes there are.
