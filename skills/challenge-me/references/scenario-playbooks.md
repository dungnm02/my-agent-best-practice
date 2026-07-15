# Scenario playbooks

Each playbook gives: the opening question, an escalation ladder (question
categories to move through, one question per round, adapting to answers),
and what counts as a "genuine attempt" for the escape hatch. Questions are
examples — rephrase them to fit the user's actual problem; never read them
out like a script.

## 1. Technical decisions

Triggers: "Should I use X?", "I'm going to use X for this", "X or Y?"

**Opening:** "Before I weigh in — what problem is this decision actually
solving, and what constraints are non-negotiable?"

**Escalation ladder:**

1. **Alternatives** — "What else did you consider, and why did you reject
   it?" Require at least two named alternatives with real rejection
   reasons. "It's popular" is not a reason; push past it.
2. **Evidence for constraints** — "You said you need <constraint> — what's
   that based on? Measured, estimated, or assumed?"
3. **Failure modes** — "How does this choice fail? What's the worst
   operational surprise it could give you in six months?"
4. **Scale and maintenance** — "What changes at 10x the load/data/team
   size? Who maintains this and what does it cost them?"
5. **Falsifiability** — "What evidence would change your mind?" A decision
   they can't imagine reversing is a belief, not a decision.

**Genuine attempt:** a stated choice plus at least one real tradeoff
comparison against an alternative.

## 2. How-do-I questions

Triggers: "How do I implement X?", "What's the best way to do X?"

**Opening:** "Explain the problem back to me in your own words — inputs,
outputs, and what makes it non-trivial."

**Escalation ladder:**

1. **Their approach first** — "Before I say anything: how would YOU attack
   this? Rough sketch is fine."
2. **Concrete walkthrough** — "Walk your approach through a concrete
   example — take <realistic input> and tell me what happens step by step."
3. **Edge cases** — "What inputs break it? Empty, huge, duplicate,
   malformed, concurrent?"
4. **Cost** — "What's the complexity or performance cost? Where's the
   bottleneck?"

**Hint ladder** (when they're genuinely stuck — require a new attempt
between levels, never skip levels):

1. Nudge — name the general area ("think about what happens on the second
   page of results").
2. Narrowed direction — name the technique category ("this is a cursor
   problem, not an offset problem").
3. Skeleton — pseudocode or the shape of the solution with gaps for them
   to fill.
4. Full answer — complete solution, with a comparison to their attempts.

**Genuine attempt:** a proposed approach, even partial or wrong.

## 3. Debugging

Triggers: "Why is this broken?", "This doesn't work", a pasted stack
trace or failing test with no analysis attached.

**Opening:** "What exactly do you observe, and what did you expect
instead? Precise symptoms, not 'it doesn't work'."

**Escalation ladder:**

1. **Delta** — "When did this last work? What changed between then and
   now — code, data, config, environment?"
2. **Hypotheses** — "Give me at least two hypotheses for the cause, ranked
   by likelihood, with your reasoning for the ranking."
3. **Cheapest experiment** — "What's the cheapest experiment that
   discriminates between your top two hypotheses?"
4. **Prediction** — "Before you run it: what exactly will the output be if
   hypothesis 1 is right? If hypothesis 2 is right?" Predicting before
   observing is the habit being trained — don't skip this round.
5. Let them run the experiment and update. Repeat 3–4 until cornered.

**Genuine attempt:** a testable hypothesis plus how they'd test it.

## 4. Code comprehension

Triggers: code was just written (by the user or by an AI) and the user
wants to move on; or the user asks to be quizzed on a piece of code.

**Opening:** "Before this counts as done — explain what this code does
end to end, in your own words, without reading it line by line."

**Escalation ladder:**

1. **Why this shape** — "Why this approach over <plausible alternative>?
   What would the alternative cost?"
2. **Edge behavior** — "What happens if <empty input / dependency fails /
   two callers hit this at once>?"
3. **Modification probe** — "What would you change to support <small
   requirement twist>? Which parts stay untouched?"

The task does not count as done until they can answer the end-to-end
explanation and the "why this shape" question. If the code was
AI-written and they can't explain it, that's the headline finding for
the debrief — the code stayed a black box.

**Genuine attempt:** an end-to-end explanation attempt in their own
words, even with gaps.
