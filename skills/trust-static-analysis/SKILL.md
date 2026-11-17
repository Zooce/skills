---
name: trust-static-analysis
description: >
  Add a deterministic check that catches a mistake the code still
  allows: a type, a compiler setting, a lint rule, a dependency rule, or
  a test. The check fails on the real past mistake, its message says
  what to do instead, and it runs with the repo's other checks. The
  static analysis layer of Lauren Tan's trust pyramid. Use when an agent
  repeats a mistake, a review comment repeats, or the user runs
  /zooce:trust-static-analysis.
---

# Static analysis layer

Static analysis is the second layer of the trust pyramid. A check gives every change the same answer, whoever wrote it. Its message reaches the agent at the moment the agent breaks the rule. A comment or a rule file cannot do that.

Source: Lauren Tan, [All Code Is Slop (Except Mine)](https://www.youtube.com/watch?v=O_zcc6Xl12g), AI Engineer CODE 2026.

## Input

One failure with a link, and the code that allowed it. If `trust-codebase` can remove the choice, do that instead. A check is the right layer when the code must keep the choice, such as an API that framework code may use and product code may not.

## The finished change

The change adds one check.

- Use the strongest kind that catches the case. A type or compiler error comes first, then an existing lint rule, then a custom lint or dependency rule, then a unit test in the check suite. A test that drives the app the way a user does is a proof for `trust-verification`, not a check for this layer.
- The error message names the file, function, or API to use instead, in one sentence. The message replaces the comment or rule file line that would otherwise say it.
- The check runs as a mise task, and the repo's task that runs every check, such as `mise run verify`, includes it. It runs on a fresh clone with no secrets. Follow `dev-tooling-mise`.
- The change fixes the existing violations. If there are too many for one small change, the check fails only when a change adds a new one, and the change records the current count.
- An allowed exception goes in the check's config with its reason, not in an inline comment.

## Done when

- The check fails on the real past mistake. Put the old code back in a throwaway copy made with `mktemp -d`, run the check, and record the failing output.
- The check passes on the current code.
- The failing output names what to use instead.
- The full check task passes. Record the command, the result, and the commit SHA.

## Report

- The failure and its link.
- The check, and why a higher layer could not stop the failure.
- The failing output on the past mistake.
- Any violations left, with their count.
