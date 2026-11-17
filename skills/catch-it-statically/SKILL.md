---
name: catch-it-statically
description: >
  Add a deterministic check that catches a mistake the code still
  allows: a type, a compiler setting, a lint rule, a dependency rule, or
  a test. The check fails on the real past mistake, its message says
  what to do instead, and it runs with the repo's other checks. Use when
  an agent repeats a mistake, a review comment repeats, or the user runs
  /zooce:catch-it-statically.
---

# Catch it statically

A check gives every change the same answer, whoever wrote it. Its message reaches the agent at the moment the agent breaks the rule. A comment or a rule file cannot do that.

## Input

One failure with a link, and the code that allowed it. If `design-it-out` can remove the choice, do that instead. Add a check when the code must keep the choice, such as an API that framework code may use and product code may not.

## The finished change

The change adds one check.

- Use the strongest kind that catches the case. A type or compiler error comes first, then an existing lint rule, then a custom lint or dependency rule, then a unit test in the check suite. A test that drives the app the way a user does belongs to `catch-with-a-proof`, not to this skill.
- The error message names the file, type, or function to use instead, in one sentence. The message replaces the comment or rule file line that would otherwise say it.
- The check runs as a mise task, and the repo's task that runs every check, such as `mise run verify`, includes it. It runs on a fresh clone with no secrets. Follow `dev-tooling-mise`.
- The change fixes the existing violations. If the pattern is already common, or there are too many violations for one small change, the check fails only when a change adds a new one, and the change records the current count.
- An allowed exception goes on the offending line with a reason, an expiry date, and a human's approval.

## Done when

- The check fails on the real past mistake. Put the old code back in a throwaway copy made with `mktemp -d`, run the check, and record the failing output.
- The check passes on the current code.
- The failing output names what to use instead.
- The full check task passes. Record the command, the result, and the commit SHA.

## Report

- The failure and its link.
- The check, and why the code still has to offer the choice.
- The failing output on the past mistake.
- Any violations left, with their count.
