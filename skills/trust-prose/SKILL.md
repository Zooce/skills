---
name: trust-prose
description: >
  Write a rule for a decision that needs judgment and that no code,
  check, or proof can enforce, and delete the rules that something else
  now enforces. Covers AGENTS.md, rule files, and skills. The prose
  layer of Lauren Tan's trust pyramid. Use when you are about to add a
  rule after correcting an agent, when a rule file grows, or when the
  user runs /zooce:trust-prose.
---

# Prose layer

Prose is the fourth layer of the trust pyramid. Nothing fails when an agent skips a rule, so keep prose small. Use it only for decisions that need judgment.

Source: Lauren Tan, [All Code Is Slop (Except Mine)](https://www.youtube.com/watch?v=O_zcc6Xl12g), AI Engineer CODE 2026.

## Input

A correction you gave an agent, with a link to the failure it corrects.

## Try a higher layer first

Write one sentence for each layer above prose that says why it cannot stop the failure:

- `trust-codebase`: why the code cannot drop the choice.
- `trust-static-analysis`: why no check can catch it.
- `trust-verification`: why no proof can show it.

If any sentence is weak, use that skill and write no rule.

## The finished change

- One rule, in the file agents read when they make that decision: the skill for the task, or AGENTS.md for the whole repo. Not a code comment, and not a new Markdown file.
- The rule names the decision, says what to do, and links the real failure.
- The rule follows `technical-writing` and `unslop`.
- The change deletes the rules in the same file that a check, a type, or a proof now enforces, and names what enforces each one.
- The change deletes rules that cannot be traced to a real failure, and Markdown that an agent wrote to justify a workaround.

## Done when

- The new rule has its failure link and the three sentences above.
- Each deleted rule is listed with what enforces it now, or why it had no source.
- `writing-review` on the change has no Act On findings.

## Report

- The rule and its failure link.
- Why each higher layer could not stop the failure.
- The rules you deleted, and why.
