# Tech-lead critique checklist

Walk these dimensions against the user's draft, one issue per round.
Skip dimensions the draft already covers — the checklist is for
finding holes, not for ceremony. Phrase every finding as a question
that lets the user find the hole themselves.

## 1. Scope and "done"

- Do the steps actually add up to the acceptance criteria, or is
  there a gap between the last step and "done"?
- Is anything in the plan not required by the ticket (scope creep)?

## 2. Order and dependencies

- Can the steps run in the stated order? What does step N need that
  only exists after step N+2?
- What's blocked on other people/teams/reviews, and is that on the
  critical path?

## 3. Existing data and state

- What happens to data created before this change ships? Migration,
  backfill, or genuinely nothing?
- Are there in-flight processes (jobs, sessions, queued events) that
  will straddle the deploy?

## 4. Failure and edge cases

- Which step is most likely to not work on the first try?
- What are the inputs nobody mentioned: empty, huge, duplicate,
  concurrent, malformed?

## 5. Testing

- How does each step get verified — and is "manually poke it" doing
  too much work in that answer?
- What existing behavior could this break, and what test proves it
  didn't?

## 6. Rollout and rollback

- Can this ship behind a flag / in stages, and does it need to?
- If it's wrong in production, what's the undo — revert, flag off,
  data repair?

## 7. Communication

- Who is surprised if this ships silently (API consumers, support,
  another team's dashboard)?
- Does anything need docs, a changelog entry, or a heads-up message?

## 8. Estimate sanity

- Which single step carries the most uncertainty, and how much of
  the estimate does it hold?
- Does the estimate include review cycles, QA, and the unknowns
  listed — or just typing time?

## Calibration

For a small ticket, dimensions 1, 4, and 5 usually carry the value —
don't force all eight. For anything touching persistent data or a
public interface, dimensions 3 and 6 are non-negotiable.
