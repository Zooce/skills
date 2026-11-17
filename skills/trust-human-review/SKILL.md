---
name: trust-human-review
description: >
  Make human review rarer and cheaper. Move each mistake a reviewer
  catches to a higher layer, so no reviewer catches it twice, and give
  reviewers evidence of what a change does instead of a diff to read.
  The human review layer of Lauren Tan's trust pyramid. Use after a
  reviewer finds a mistake, after review misses one that later breaks,
  when review is the bottleneck, or when the user runs
  /zooce:trust-human-review.
---

# Human review layer

Human review is the narrowest layer of the trust pyramid, and the most expensive. A reviewer who reads every line becomes the limit on how much work ships. Improve this layer by giving it less to catch and better evidence to judge.

Source: Lauren Tan, [All Code Is Slop (Except Mine)](https://www.youtube.com/watch?v=O_zcc6Xl12g), AI Engineer CODE 2026.

## Input

A finding from a human review with its link, or a merged change that review missed and that later broke.

## The finished change

- **The catch moves up.** For each finding, the change applies the skill for the highest layer that could have caught it: `trust-codebase`, `trust-static-analysis`, `trust-verification`, or `trust-prose`. The finding links to that change.
- **Reviewers get evidence.** For a change a person must judge, the pull request shows what the change does. Include screenshots or a recording of each changed state, the terminal output, and the mise tasks run with their results. For a change in behavior, add a diagram of the states and transitions from the user's point of view. The reviewer compares the diagram with the recording to judge whether the agent built what was asked. `ui-review-checklist` lists the states to cover.
- **What stays human is named.** Each finding that stays with people says why no higher layer can catch it, such as product intent, taste, or a risk that the owner keeps. `security-review` covers the risks that stay with the owner.

Read the code sometimes, even when the checks pass. Code that stays becomes the example the next agent copies. When a review shows a workaround or an old pattern that could spread, send it to `trust-codebase`.

## Done when

- Each finding links to the change at a higher layer, or gives the reason it stays human.
- The pull request evidence lets a reviewer judge the behavior without running the code.

## Report

- Each finding, the layer it moved to, and the link.
- The findings that stay human, and why.
