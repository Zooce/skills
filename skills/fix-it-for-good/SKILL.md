---
name: fix-it-for-good
description: >
  When an agent is corrected or a problem recurs, find the mistake
  types that have happened twice, then apply the strongest skill that
  can stop each one: design it out, catch it statically, catch with a
  proof, fix a judgment, or learn from review, in that order. Use when an agent repeats a
  mistake, a bug gets through, a reviewer corrects a change, or the
  user runs /zooce:fix-it-for-good.
---

# Fix it for good

Find the mistake types that keep repeating, then apply the strongest skill that can stop each one.

## Find the mistake types

Read recent commits, reverts, review comments, agent rules such as AGENTS.md, and comments that explain workarounds. Group the mistakes into types. A type counts once it has happened twice.

If you were pointed at one failure, include it. Count the type if this is the second time it has happened.

## Pick the skill

For each type, try these skills in this order. Stop at the first one that can stop the failure.

1. `design-it-out`, when the code offers a choice that allowed the failure, and that choice can be removed.
2. `catch-it-statically`, when the code must keep the choice, and a type, lint, compiler, or unit test can catch the mistake.
3. `catch-with-a-proof`, when a type, lint, or unit test cannot see the failure: the running app, a rule across states, or a formal model.
4. `fix-a-judgment`, when the decision needs judgment, and no code, check, or proof can enforce it.
5. `learn-from-review`, last, when a person must still judge the change, when review itself is the bottleneck, or when reviewers need evidence of what a change does.

Write one sentence for each stronger skill that says why it cannot stop the failure. Then apply the skill you picked.

A type can need two skills. For example, a directory split in `design-it-out` needs a lint rule in `catch-it-statically` to enforce it. A lint rule can block new uses of an old API while callers move off it, and the code change then deletes the API.

## Report

- Each mistake type, and the evidence that it happened at least twice.
- The skill you picked, and why each stronger skill would not work.
- Any second skill the type also needs, and why.
- The result of the skill you applied.
