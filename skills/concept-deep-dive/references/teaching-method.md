# Teaching method

## Gauging questions

The goal is to find the *edge* of understanding — the last thing they
can explain confidently. Ask in increasing depth until an answer gets
vague, then teach from there:

1. Recognition: "Where have you seen X used?"
2. Mechanism: "What do you think X actually does under the hood?"
3. Prediction: "What would happen if X weren't there / were removed?"

A confident wrong answer is more useful than "I don't know" — teach
against the misconception directly and name it as one.

## The three layers

**Intuition.** One analogy or mental model, chosen so the mechanics
feel inevitable afterward (an index is a book's index; an event loop
is a single waiter working many tables). State explicitly where the
analogy will break before moving on — a leaky analogy taught as exact
becomes next month's misconception.

**Mechanics.** The real behavior, concrete: what's stored, what
happens step by step, what the actual costs are. Prefer walking
through one small concrete example over describing the general case.
Numbers beat adjectives ("a B-tree lookup touches ~3–4 pages" beats
"it's fast").

**Tradeoffs.** Every concept earns its complexity somewhere and loses
somewhere else: write cost of indexes, staleness of caches, the
debugging cost of async. Cover when NOT to use it, and one question
seniors genuinely disagree about — that's where real understanding
shows.

Advance between layers only after a checking question lands ("so why
would writes get slower?"). If it doesn't land, re-teach the current
layer differently instead of pushing on.

## Exercise design

- One exercise, checkable, completable in under ~10 minutes.
- Tie it to their world: their table, their endpoint, their bug — or
  a stripped-down replica of it. Abstract textbook exercises transfer
  worse.
- Prefer *predict-then-verify* shapes ("predict what EXPLAIN will
  show, then run it") — the prediction forces the mental model out
  where you can inspect it.
- Review honestly: a right answer with wrong reasoning gets follow-up,
  not a pass.

## Failure modes to avoid

- **The lecture wall**: dumping all three layers in one message. One
  layer per message, question between.
- **Teaching the general case**: covering variants and edge cases the
  current task doesn't need. Name them, skip them.
- **Analogy worship**: never leaving the analogy for the mechanics.
  The analogy is scaffolding; the mechanics are the building.
- **Skipping the connect-back**: without step 4 the dive stays
  abstract knowledge; the one-sentence "why the fix works" is what
  binds it to experience.
