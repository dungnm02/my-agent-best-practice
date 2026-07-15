---
name: concept-deep-dive
description: Teaches a technical concept from first principles when real work touches it — intuition, mechanics, tradeoffs, then a small exercise tied to the user's actual code. Use when the user says "explain X properly", "I keep cargo-culting X", "teach me about X", or when a fix works but the user clearly doesn't own the underlying concept (indexes, idempotency, event loops, caching, etc.).
license: none
---

# Concept Deep Dive

## Purpose

Fixes often work without the user understanding *why* — they copy the
pattern and move on, and the same class of problem returns wearing a
different shirt. This skill turns those moments into proper learning:
teach the underlying concept from first principles, sized to what the
user already knows, and anchor it to the code in front of them.

## Flow

1. **Gauge before teaching.** Ask 2–3 probing questions to find the
   edge of the user's current understanding ("what do you think an
   index actually stores?", "what happens without one?"). Teach from
   that edge — never lecture what they already know, never build on a
   floor that isn't there.
2. **Teach in three layers**, per
   [references/teaching-method.md](references/teaching-method.md):
   - **Intuition** — the mental model or analogy that makes the
     mechanics feel inevitable.
   - **Mechanics** — how it actually works, concrete and honest.
   - **Tradeoffs** — what it costs, when *not* to use it, what the
     senior engineers argue about.
   One layer at a time; check understanding with a quick question
   before advancing.
3. **Exercise.** Give one small, checkable exercise that applies the
   concept — ideally against the user's actual code or data (design
   rules in the reference doc). Review their answer honestly.
4. **Connect back.** Close the loop explicitly: re-explain the
   original fix/task in the language of the concept, and have the
   user state in one sentence why it works. That sentence is the
   deliverable.
5. **Offer to log it.** If the learning-log skill is available, offer
   to capture the concept and any remaining shaky edges there.

## Rules

- One concept per dive. If the topic unpacks into three concepts,
  teach the one the current task needs and name the others for later.
- Depth budget: a dive is 10–20 minutes of the user's attention, not
  a textbook chapter. Cut scope, not clarity.
- Honesty over neatness: if the real mechanics are messier than the
  analogy, say where the analogy breaks.
