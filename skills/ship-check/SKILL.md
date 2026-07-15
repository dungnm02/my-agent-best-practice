---
name: ship-check
description: Runs a pre-PR self-review ritual — the user reviews their own diff and predicts reviewer feedback first, then the agent reviews and compares findings against the predictions. Use when the user says "check my PR", "review before I open this", "is this ready to ship?", or is about to open/submit a pull request.
license: none
---

# Ship Check

## Purpose

Every PR is a chance to practice the reviewer's eye — unless the agent
reviews it first, in which case the user learns to wait for feedback
instead of developing judgment. This skill reverses the order: the
user self-reviews and predicts what a reviewer would say, *then* the
agent reviews, and the gap between prediction and findings is the
lesson.

## Flow

1. **Scope the diff.** Establish what's being shipped (branch diff
   against the base, e.g. `git diff main...HEAD`) and read nothing
   else yet — no opinions before the user commits to theirs.
2. **User predicts first.** Have them re-read their own diff and list
   their predictions: what will a reviewer flag? What are they least
   confident about? Minimum three predictions; "it's fine" is not a
   self-review. Do not reveal anything you've noticed until their
   list is in.
3. **Agent reviews.** Now review the diff properly against
   [references/review-checklist.md](references/review-checklist.md).
   Be a real reviewer: severity-ranked findings with file/line
   references, including anything genuinely good worth keeping.
4. **Compare.** Present findings in three buckets:
   - **You caught it** — their prediction matched a real finding.
   - **You missed it** — findings not in their predictions (the
     lesson lives here; explain *how* a reviewer spots each one).
   - **False alarm** — things they predicted that are actually fine,
     and why.
5. **Fix and finish.** Agree which findings block shipping vs. can
   ride. The user decides; you flag only true blockers as
   non-negotiable (broken behavior, security, data loss). Then check
   the wrapping: commit messages and PR description per the
   checklist's final section.
6. **Tally and log.** Close with the caught/missed count and the one
   category they most often miss. Offer to log recurring misses via
   the learning-log skill if available.

## Rules

- Never start reviewing before the user's predictions are in — the
  ritual's entire value is the commitment step.
- Findings need evidence: file, line, and the failure it causes. No
  vibes-based "this could be cleaner" without saying cleaner *how*.
- Calibrate to the diff: a three-line fix gets a 2-minute ship-check,
  not the full ceremony. Steps 2 and 4 survive even in the short form.
