---
name: trust-pyramid
description: >
  Pick the layer of Lauren Tan's trust pyramid where a failure should be
  stopped, then apply that layer's skill. Use when an agent repeats a
  mistake, a bug gets through, a reviewer corrects a change, or the user
  runs /zooce:trust-pyramid.
---

# Trust pyramid

The trust pyramid is upside down. The widest layer is at the top, and cost rises toward the tip. Stop each failure at the highest layer that can stop it. The layers below are backstops.

Source: Lauren Tan, [All Code Is Slop (Except Mine)](https://www.youtube.com/watch?v=O_zcc6Xl12g), AI Engineer CODE 2026.

| Layer | What it holds | Skill |
| ----- | ------------- | ----- |
| Codebase | API design, architecture, refactoring | `trust-codebase` |
| Static analysis | Lints, types, compilers, tests, CI | `trust-static-analysis` |
| Verification | Runtime and formal proofs | `trust-verification` |
| Prose | Rules, skills, AGENTS.md | `trust-prose` |
| Human review | People judge the change | `trust-human-review` |

## Pick the layer

Start at the codebase layer and move down only when the layer cannot stop the failure. Write one sentence for each layer you pass that says why it cannot.

A failure can need two layers. For example, a directory split at the codebase layer needs a lint rule at the static analysis layer to enforce it. A lint rule can block new uses of an old API while callers move off it, and the codebase change then deletes the API.

## Report

- The failure and its link.
- The layer you picked.
- One sentence for each higher layer on why it cannot stop the failure.
- The result of that layer's skill.
