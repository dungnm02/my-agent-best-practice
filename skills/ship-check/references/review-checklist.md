# Reviewer checklist

Review the diff against these categories. Report findings
severity-first. Every finding: file, line, what breaks or degrades,
and (in the compare step) how a reviewer's eye catches it.

## Blocking — must fix before ship

- **Correctness**: logic errors, inverted conditions, off-by-ones,
  unhandled null/empty/error paths, race conditions on shared state.
- **Data safety**: writes that can lose or corrupt data, missing
  transactions where steps must be atomic, destructive migrations
  without backfill/rollback.
- **Security**: injection, unescaped output, secrets in the diff,
  authz checks missing on new endpoints, trusting client input.
- **Breaking changes**: public API/contract changes without
  versioning or coordination, behavior changes existing callers
  depend on.

## Should fix — push back unless justified

- **Tests**: new behavior with no test that fails without the change;
  tests that assert the implementation rather than the behavior;
  edge cases from the code (empty, duplicate, concurrent) absent
  from the tests.
- **Error handling**: swallowed exceptions, errors logged without
  context, failure paths that leave state half-updated.
- **Performance**: N+1 queries, unbounded loops/reads on data that
  grows, work inside loops that belongs outside.

## Worth raising — reviewer's discretion

- **Naming and clarity**: names that lie about behavior, functions
  doing more than they say, magic numbers without a named constant.
- **Dead weight**: commented-out code, unused imports/params, debug
  leftovers (prints, temporary flags).
- **Consistency**: new code that ignores the pattern the surrounding
  code established (error style, layering, naming scheme).

## The wrapping — check last

- **Commit messages**: does each say *why*, not just *what*? Would
  `git log --oneline` of this branch make sense to a stranger?
- **PR description**: states the problem, the approach, and how it
  was tested; calls out anything reviewers should look at hardest;
  no "misc fixes".
- **Diff hygiene**: no unrelated changes riding along (formatting
  sweeps, drive-by refactors) — those belong in their own PR.

## Spotting guidance (for the compare step)

When explaining a missed finding, name the reviewer habit that
catches it, e.g.:

- Correctness: "read every `if` asking what makes it false".
- Data safety: "find every write, then ask what happens if the next
  line never runs".
- Tests: "imagine deleting the change — which test goes red?"
- Breaking changes: "list who calls this today, not who should".
