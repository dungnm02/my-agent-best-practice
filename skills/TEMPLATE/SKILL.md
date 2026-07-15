---
name: TEMPLATE
description: One or two sentences, third person, stating what this skill does AND when an agent should use it (the trigger conditions/keywords). This is the only field agents see before deciding to load the skill, so be specific.
license: none
---

# Skill Title

## Purpose

What problem this skill solves and why it exists. Keep this section short —
this file is loaded in full once the skill is triggered, so every line has a
cost.

## When to use

- Bullet the concrete situations, phrases, or file types that should trigger
  this skill.
- Bullet what's explicitly out of scope, if that's not obvious.

## Instructions

Step-by-step guidance for the agent to follow. Prefer imperative, checkable
steps over prose. If a step is long or only needed sometimes, move it to
`references/` and link it instead of inlining it here (progressive
disclosure — keep SKILL.md itself small).

1. Step one.
2. Step two.
3. ...

## Resources

- `scripts/` — executable helpers the agent can run directly (e.g. `python
  scripts/foo.py`). Only add a script if deterministic code is more
  reliable than the agent improvising it each time.
- `references/` — supplementary docs loaded on demand (API specs, schemas,
  detailed how-tos). Reference them by relative path from this file; don't
  paste their contents here.
- `assets/` — non-code files used in output (templates, boilerplate,
  fixtures, images).

Delete any of the three subdirectories you don't end up using.
