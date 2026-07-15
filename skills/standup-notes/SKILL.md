---
name: standup-notes
description: Turns recent git history and session context into crisp standup or worklog notes in Yesterday / Today / Blockers format. Use when the user says "write my standup", "what did I do yesterday?", "summarize my work this week", or needs a worklog/status update for a meeting or report.
license: none
---

# Standup Notes

## Purpose

Reconstructing "what did I actually do?" from memory every morning
wastes time and drops the details worth mentioning. This skill builds
the notes from evidence — git history plus session context — in the
shape a standup actually wants.

## Flow

1. **Gather evidence.** Pull recent history for the working period —
   last workday by default (mind Mondays: "yesterday" means Friday),
   or the range the user names:
   - `git log --since=... --author=... --all --oneline` for commits
     across branches (confirm the author identity via `git config
     user.email` if unsure).
   - Current branch status: open work, uncommitted changes, PRs
     mentioned in the session.
   - Anything discussed this session that never reached a commit
     (investigations, reviews, meetings the user mentions).
2. **Draft in standup shape** (or the format the user names):
   - **Yesterday** — what moved, grouped by task/ticket, not by
     commit. "Finished pagination for orders API (PR #42, in
     review)" — never a commit-hash list.
   - **Today** — inferred from open branches and session context;
     mark inferences so the user can correct them.
   - **Blockers** — only real ones (waiting on review, missing
     access, unanswered question). No blockers is a fine answer.
3. **Deliver tight.** Three to six bullets total, plain language a
   non-engineer in the meeting can follow, ticket/PR references where
   they exist. No file paths, no jargon dumps.
4. **Let the user own it.** They know about the meeting and the
   hallway conversation git can't see — invite one correction pass,
   apply it, done.

## Rules

- Evidence over memory: if git and the user's recollection disagree,
  show the evidence and let them decide.
- Never editorialize progress ("great progress on...") — state what
  happened; the user sets their own tone.
- Weekly/monthly summaries: same flow, grouped by theme instead of
  by day, still ruthlessly short.

## Resources

None — this skill is a single SKILL.md, which is a perfectly valid
skill shape when there's nothing to progressively disclose.
