---
name: add-rule
description: >
  Write a rule for a decision that needs judgment and that no code,
  check, or proof can enforce, and delete the rules that something else
  now enforces. Keep a table in AGENTS.md that pairs each rule with
  what enforces it. Use when you are about to add a rule after
  correcting an agent, when a rule file grows, or when the user runs
  /zooce:add-rule.
---

# Add a rule

Nothing fails when an agent skips a rule, so keep prose small. Use it only for decisions that need judgment.

## Input

A correction you gave an agent, with a link to the failure it corrects.

## Try another skill first

Write one sentence for each of these skills that says why it cannot stop the failure:

- `change-the-code`: why the code cannot drop the choice.
- `add-check`: why no check can catch it.
- `add-proof`: why no proof can show it.

If any sentence is weak, use that skill and write no rule.

If the correction repeats a rule in AGENTS.md and the table shows nothing enforces it, do not write another rule. Apply `change-the-code`, `add-check`, or `add-proof` in the same change, and update the table.

## The finished change

- One rule, in the file agents read when they make that decision: the skill for the task, or AGENTS.md for the whole repo. Not a code comment, and not a new Markdown file.
- The rule names the decision, says what to do, and links the real failure.
- The rule follows `technical-writing` and `unslop`.
- A table in AGENTS.md pairs each rule with what enforces it, including rules that live in a skill. Each row names the rule and what enforces it: a code change, a check, a proof, or this rule. Add a row when you add a rule. Drop a row once the mistake cannot happen.
- The change deletes the rules in the same file that a check, a type, or a proof now enforces, and names what enforces each one.
- The change deletes rules that cannot be traced to a real failure, and Markdown that an agent wrote to justify a workaround.

## Done when

- The new rule has its failure link and the three sentences above, or the change moved an unenforced rule to a stronger fix and updated the table.
- The AGENTS.md table matches the rules that remain.
- Each deleted rule is listed with what enforces it now, or why it had no source.
- `writing-review` on the change has no Act On findings.

## Report

- The rule and its failure link, or the stronger fix you applied instead.
- Why each of those skills could not stop the failure, when you wrote a rule.
- The rules you deleted, and why.
- The table rows you added, updated, or dropped.
