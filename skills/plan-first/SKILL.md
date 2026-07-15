---
name: plan-first
description: Has the user draft a ticket breakdown (steps, unknowns, risks, estimate) before any code is written, then critiques it like a tech lead in sprint planning. Use when the user starts a new ticket/feature/task, says "help me break this down", "plan this ticket with me", or is about to code something non-trivial with no written plan.
license: none
---

# Plan First

## Purpose

Decomposition and estimation are the two skills juniors are worst at
and the two that AI most silently takes over. This skill keeps the
drafting pen in the user's hand: they break the work down, the agent
critiques like a tech lead — so every ticket becomes planning practice
instead of planning outsourcing.

## Flow

1. **State the ticket.** Have the user describe the task and its
   acceptance criteria in their own words. If they can't state what
   "done" means, that's the first gap — resolve it before planning.
2. **User drafts the breakdown.** They write it, you wait. Format:
   - Steps, in order, each small enough to verify.
   - Unknowns — questions they can't answer yet, and who/what answers them.
   - Risks — what could blow up the estimate or break existing behavior.
   - A rough estimate.
3. **Critique like a tech lead.** Walk the dimensions in
   [references/critique-checklist.md](references/critique-checklist.md),
   raising ONE issue at a time as a question, not a correction ("what
   happens to in-flight orders when this deploys?"). Let the user
   amend the draft themselves after each round. Stop when the plan
   survives the checklist or the user calls it good enough.
4. **Converge.** Read back the final plan: steps, resolved unknowns,
   accepted risks, estimate. This version is theirs to paste into the
   ticket. Only now do you add anything the questioning never surfaced,
   labeled as your addition.
5. **Optional retro.** If the user returns after shipping, compare
   actual vs estimate and where the surprises came from. Offer to log
   recurring estimation misses via the learning-log skill if available.

## The gate

Do not produce a breakdown yourself before the user has drafted one
genuine attempt — a real ordered list of steps, however flawed. If
asked to "just plan it":

- Before a genuine attempt: decline once and hand back the smallest
  starter question ("what's the very first thing that has to change?").
- After a genuine attempt: you may co-write the rest, but their draft
  stays the skeleton — amend it, don't replace it.

## Rules

- Critique the plan, not the plan's author. Questions, not verdicts.
- Missing-step hints point at the checklist dimension ("anything need
  to happen to existing data?"), never at the answer.
- Estimates are the user's. Challenge the reasoning behind one, never
  substitute your own number until converging in step 4.
