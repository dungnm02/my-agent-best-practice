---
name: code-tour
description: Gives the user a structured guided tour of an unfamiliar code area — the map, one flow traced end-to-end, key invariants — then has them trace a second path themselves to prove understanding. Use when the user says "give me a tour of X", "walk me through this module/repo", "help me understand this codebase", or is onboarding onto code they didn't write.
license: none
---

# Code Tour

## Purpose

Reading unfamiliar code is a core engineering skill, and "explain this
file to me" teaches none of it. This skill runs a guided tour that
models *how* to read a codebase — top-down, flow-first, invariants
last — and ends with the user navigating on their own, so the skill
transfers instead of creating dependence.

## Flow

1. **Scope the tour.** Agree with the user on the area (a module, a
   feature, a directory) and what they need from it — "I have to add a
   feature here" tours differently than "I'm on call for this". Keep
   scope to something coverable in one sitting; offer to split
   anything bigger.
2. **Explore silently.** Read the code yourself first: entry points,
   the main flow, data shapes, tests. Don't narrate the exploration.
3. **Present the tour in three layers**, following
   [references/tour-structure.md](references/tour-structure.md):
   - Layer 1 — the map: what lives where, entry points, boundaries.
   - Layer 2 — one core flow traced end-to-end with real file/function
     references.
   - Layer 3 — invariants and gotchas: what must stay true, what bites.
   Pause after each layer and let the user ask questions before moving on.
4. **Finale — they drive.** Pick a second, different path through the
   same area and have the user trace it themselves, file by file,
   while you follow along. Ask checking questions from the reference
   doc; correct wrong turns by pointing at evidence in the code ("look
   at what that function returns"), not by giving the route.
5. **Wrap up** with a three-bullet summary the user writes (not you):
   where things live, how the main flow works, what to be careful of.
   Offer to log it via the learning-log skill if that skill is
   available.

## Rules

- Every claim in the tour points at a real location (`path:line` or
  function name) — no vague "somewhere in the service layer".
- Tour the code that exists, including its warts. If something is
  confusing or badly named, say so; pretending the code is clean
  teaches the user to distrust their own confusion.
- The finale is not optional. A tour without the user driving is just
  a lecture, and lectures don't stick.
