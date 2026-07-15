---
name: learning-log
description: Maintains a durable markdown journal of things the user learned and reasoning gaps they discovered, and quizzes them on past entries to make the knowledge stick. Use when the user says "log what I learned", "add this to my learning log", after a debrief surfaces gaps worth keeping, or when the user asks to be quizzed/reviewed on their log ("quiz me on my log", "review my learning log").
license: none
---

# Learning Log

## Purpose

Lessons learned during work sessions evaporate unless they are captured
and revisited. This skill keeps a persistent journal of TILs and
reasoning gaps, then resurfaces old entries as quiz questions so the
user actually retains them — spaced repetition for engineering judgment.

## The log file

Default location: `learning-log.md` at the project root. The first time
this skill runs in a project, confirm the path with the user (they may
want one shared log outside the repo instead of per-project logs) and
then stick with it. Create the file from the template in
[references/log-format.md](references/log-format.md) if it doesn't exist.

## Mode 1: Capture

Trigger: the user asks to log something, or a learning moment just
happened (e.g. a debrief from another skill surfaced gaps).

1. Draft the entry using the template in
   [references/log-format.md](references/log-format.md): date, context,
   TILs, gaps, each gap with `review-status: new`.
2. Write TILs and gaps in the user's own framing where possible — ask
   them to state the lesson in one sentence before writing it down.
   Their words, not yours, are what they'll be quizzed on later.
3. Append the entry to the log. Never rewrite or delete past entries.
4. Keep it to 30 seconds of effort — a few bullets, not an essay.

## Mode 2: Review

Trigger: the user asks to be quizzed or to review the log.

1. Read the log and select 3–5 items using the heuristic in
   [references/log-format.md](references/log-format.md) — shaky items
   first, then oldest unreviewed, then anything not seen recently.
2. Quiz one item at a time. Ask the user to explain the concept or
   re-derive the lesson from scratch — don't show them the entry first.
   Follow up once if the answer is shallow.
3. After each item, update its `review-status` (`solid` or `shaky`) and
   `last-reviewed` date in place. Statuses are the only thing you ever
   edit in old entries.
4. Close with a one-line summary: what's solid now, what to re-review
   next time.

## Rules

- The log belongs to the user. Read it back on request, never judge it.
- If another skill (e.g. a debrief) produced the gaps, condense them —
  don't paste a transcript into the log.
- If the log grows past ~50 entries, offer to archive solid entries to
  a `learning-log-archive.md` file rather than deleting them.
