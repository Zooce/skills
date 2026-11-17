---
name: learn-from-review
description: >
  Make human review rarer and cheaper. Move each mistake a reviewer
  catches into a code change, a check, a proof, or a written rule, so no
  reviewer catches it twice, and give reviewers evidence of what a
  change does instead of a diff to read. Use after a reviewer finds a
  mistake, after review misses one that later breaks, when review is the
  bottleneck, or when the user runs /zooce:learn-from-review.
---

# Learn from review

A reviewer who reads every line becomes the limit on how much work ships. Give review less to catch and better evidence to judge.

`code-review` reviews the diff in a change. This skill moves a catch into a lasting change and gives the reviewer evidence.

## Input

A finding from a human review with its link, or a merged change that review missed and that later broke.

## The finished change

- **The catch becomes a lasting change.** For each finding, apply the skill that can catch it next time: `design-it-out`, `catch-it-statically`, `catch-with-a-proof`, or `fix-a-judgment`. The finding links to that change.
- **Reviewers get evidence.** For a change a person must judge, the pull request shows what the change does. Include screenshots or a recording of each changed state, the terminal output, and the mise tasks run with their results. For a change in behavior, add a diagram of the states and transitions from the user's point of view. The reviewer compares the diagram with the recording to judge whether the agent built what was asked. `ui-review-checklist` lists the states to cover.
- **What stays human is named.** Each finding that stays with people says why a code change, a check, a proof, or a written rule cannot catch it, such as product intent, taste, or a risk that the owner keeps. `security-review` covers the risks that stay with the owner.

Read the code sometimes, even when the checks pass. Code that stays becomes the example the next agent copies. When a review shows a workaround or an old pattern that could spread, send it to `design-it-out`.

## Done when

- Each finding links to the change that will catch it next time, or gives the reason it stays human.
- The pull request evidence lets a reviewer judge the behavior without running the code.

## Report

- Each finding, the skill it moved to, and the link.
- The findings that stay human, and why.
