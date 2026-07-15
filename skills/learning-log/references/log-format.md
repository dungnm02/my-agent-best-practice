# Log format and review heuristic

## File template

When creating a fresh log file, start it with:

```markdown
# Learning Log

Entries are append-only. Only `review-status` and `last-reviewed`
fields are ever edited after the fact.
```

## Entry template

Append each entry in this exact shape so review mode can parse it:

```markdown
## 2026-07-15 — <short context, e.g. "pagination endpoint for orders API">

**TIL:**
- <one-sentence lesson, in the user's own words>

**Gaps:**
- <gap statement> <!-- review-status: new | last-reviewed: never -->
- <gap statement> <!-- review-status: new | last-reviewed: never -->
```

Rules:

- One entry per session/topic, headed by date + context. Multiple
  entries on the same date are fine.
- TILs are settled knowledge — captured for reference, not quizzed.
- Gaps are the quizzable items. Each carries an HTML comment with
  `review-status` and `last-reviewed` so the log stays readable as
  plain markdown.
- Valid `review-status` values: `new` → never quizzed; `shaky` →
  quizzed, answer was weak; `solid` → quizzed, answer held up.

## Review-selection heuristic

Pick 3–5 gap items in this priority order:

1. All items marked `shaky` (cap at 3 — don't make review sessions
   demoralizing).
2. Oldest items marked `new`.
3. Items marked `solid` whose `last-reviewed` is more than ~3 weeks
   old (spot-check that solid stayed solid).

Skip items reviewed within the last 3 days regardless of status.

## Quizzing an item

- Ask the user to explain or re-derive the item cold — do not show
  the entry text first.
- Grade honestly: a memorized phrase without the underlying "why" is
  still `shaky`. One follow-up "why does that work?" usually reveals
  which it is.
- After grading, update the item's comment in place, e.g.:
  `<!-- review-status: solid | last-reviewed: 2026-07-15 -->`
- Never edit the gap statement itself; if the user's understanding has
  evolved, add a TIL to a new entry instead.

## Archiving

When offering to archive (log past ~50 entries): move whole entries
where every gap is `solid` into `learning-log-archive.md`, preserving
their text verbatim. Never archive entries with `new` or `shaky` items.
