---
name: prose-rules
description: >
  Write a rule for a decision that needs judgment and that no code,
  check, or proof can enforce, and delete the rules that something else
  now enforces. Covers AGENTS.md, rule files, and skills. Use when you
  are about to add a rule after correcting an agent, when a rule file
  grows, or when the user runs /zooce:prose-rules.
---

# Prose rules

Nothing fails when an agent skips a rule, so keep prose small. Use it only for decisions that need judgment.

## Input

A correction you gave an agent, with a link to the failure it corrects.

## Try another skill first

Write one sentence for each of these skills that says why it cannot stop the failure:

- `design-out-problems`: why the code cannot drop the choice.
- `static-checks`: why no check can catch it.
- `agent-proofs`: why no proof can show it.

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
- Why each of those skills could not stop the failure.
- The rules you deleted, and why.
