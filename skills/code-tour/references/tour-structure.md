# Tour structure

## Layer 1 — The map

Goal: the user can answer "where would X live?" without grep.

Include:
- The 3–7 places that matter (directories/modules) and one line on
  each one's job. Not every directory — the ones this tour's purpose
  touches.
- Entry points: where execution enters this area (HTTP routes, CLI
  commands, queue consumers, cron, public API of the module).
- Boundaries: what this area owns vs. what it calls out to (DB, other
  services, shared libs).

Omit: build config, generated code, historical dead ends — unless the
tour's purpose touches them.

## Layer 2 — One core flow, end to end

Goal: the user has seen one complete request/operation travel through
the map, so the map becomes real.

- Choose the flow most central to the user's stated purpose.
- Trace it as a numbered sequence: each step names the file and
  function, states what it does in one sentence, and shows the one
  data shape that matters at that point (params in, value out).
- 5–10 steps. If the honest trace is longer, compress the boring
  middle ("three mapper functions massage this into shape X") and
  spend the steps on branch points and side effects.
- Flag where state changes (DB writes, cache, events emitted) — those
  are the steps that matter on call.

## Layer 3 — Invariants and gotchas

Goal: the user knows what will break if they change things naively.

Include only items with evidence in the code or tests:
- Invariants: orderings that must hold, fields that must never be
  null past a certain point, idempotency assumptions, transactional
  boundaries.
- Gotchas: misleading names, functions that do more than they say,
  copy-paste near-duplicates, places where the tests encode a
  requirement the code doesn't make obvious.

Three to six items. A gotcha list longer than that means the scope
was too big.

## Finale — checking questions

Have the user trace a second path (a different endpoint, the error
branch of the same flow, the write path if the tour showed the read
path). While they drive, use questions like:

- "Where does execution go next? What made you pick that file?"
- "What's in the payload at this point?"
- "This step fails — what does the caller see?"
- "You need to add field X to the response — which layers do you
  touch?"

Correcting wrong turns: point at the evidence ("check the return type
of that function") rather than naming the right file. The habit being
trained is navigating by evidence, not memorizing this codebase.
