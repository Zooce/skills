---
name: prevent-repeats
description: >
  When an agent is corrected or a problem recurs, pick the skill that
  fits: change the code so the problem cannot happen, add a static
  check, add a proof agents run, write a rule, or improve human review.
  Then apply that skill. Use when an agent repeats a mistake, a bug
  gets through, a reviewer corrects a change, or the user runs
  /zooce:prevent-repeats.
---

# Prevent repeats

Pick the skill that fits the failure, then apply it.

## Pick the skill

Match the failure to one of these skills:

| When this is the case | Skill |
| --------------------- | ----- |
| The code offers a choice that allowed the failure, and that choice can be removed | `change-the-code` |
| The code must keep the choice, and a type, lint, compiler, or unit test can catch the mistake | `add-check` |
| A type, lint, or unit test cannot see the failure: the running app, a rule across states, or a formal model | `add-proof` |
| The decision needs judgment, and no code, check, or proof can enforce it | `add-rule` |
| A reviewer caught the mistake, review missed one that later broke, or review is the bottleneck | `prep-review` |

Write one sentence that says why the skill you picked fits.

A failure can need two skills. For example, a directory split in `change-the-code` needs a lint rule in `add-check` to enforce it. A lint rule can block new uses of an old API while callers move off it, and the code change then deletes the API.

## Report

- The failure and its link.
- The skill you picked, and the sentence that says why it fits.
- Any second skill the failure also needs, and why.
- The result of the skill you applied.
