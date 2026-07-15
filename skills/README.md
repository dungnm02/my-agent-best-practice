# Skills

This directory holds reusable **Agent Skills** — self-contained folders that
package instructions (and optionally scripts/references/assets) an AI agent
can load on demand. The format is agent-agnostic: any tool that implements
the open Skill spec (Claude Code, Claude Desktop/apps, the Claude API skills
feature, etc.) can consume a skill folder from here as-is, no adaptation
needed.

## Layout

```
skills/
  <skill-name>/
    SKILL.md          # required
    scripts/           # optional — executable helpers
    references/         # optional — docs loaded on demand
    assets/              # optional — templates/files used in output
```

Each skill is a directory named after the skill (lowercase, hyphenated).
Everything the agent needs lives inside that one directory.

## Available skills

- [`challenge-me`](challenge-me/SKILL.md) — interrogates you like a skeptical
  senior engineer before giving answers, to build independent engineering
  judgment instead of AI reliance.

## Creating a new skill

1. Copy `skills/TEMPLATE/` to `skills/<your-skill-name>/`.
2. Fill in the frontmatter in `SKILL.md`:
   - `name` — must match the directory name, lowercase-hyphenated, ≤ 64 chars.
   - `description` — third person, states **what** the skill does and
     **when** to use it. This is the only part of the skill an agent sees
     before deciding to load it in full, so front-load the trigger
     keywords/situations.
3. Write the instructions in the body. Keep `SKILL.md` itself short — once
   triggered, the whole file loads into context. Move long or
   rarely-needed material into `references/` and link to it instead of
   inlining it.
4. Delete whichever of `scripts/`, `references/`, `assets/` you don't use.

## Principles

- **Progressive disclosure**: only the `name` + `description` need to be
  cheap to scan up front; the body and any `references/` files are loaded
  only when the skill actually triggers.
- **One skill, one job**: keep scope narrow so the `description` can be
  precise and agents don't misfire on unrelated tasks.
- **Prefer scripts over prose for deterministic steps**: if a step is "run
  this exact command/transform", put it in `scripts/` rather than asking
  the agent to reproduce it from instructions each time.
- **No agent-specific lock-in**: don't assume a particular tool's file
  layout (e.g. `.claude/`) inside a skill's own instructions — the same
  folder should work wherever the Skill spec is supported.
