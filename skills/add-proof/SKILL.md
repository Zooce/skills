---
name: add-proof
description: >
  Add a proof that agents run: a script that drives the running app the
  way a user does and saves evidence, a property-based test, or a formal
  model or proof. The proof fails on the real past failure and runs with
  one mise command. Use when a bug got past every static check, when a
  change worked but was not what the user asked for, or when the user
  runs /zooce:add-proof.
---

# Add a proof

A proof checks what a type, lint, or unit test cannot see: how the running app behaves, whether a rule holds across every state, and whether the agent understood what the user asked for.

`qa-verification` proves one change. This skill adds a proof that every later agent runs the same way.

## Input

One failure with a link that a type, lint, or unit test did not catch. Examples are a bug seen only in the running app, a rule that broke in one order of events, or a feature built to the wrong idea of the request. If a type, lint, or unit test can catch it, use `add-check` instead.

## The finished change

The change adds one proof. Use the kind that catches the failure with the least new work.

- **Runtime proof.** A script starts the app, drives the feature the way a user does, and saves evidence: a screenshot, terminal output, a response body, or the rows written. It uses stable handles such as accessible names, routes, and prompt strings, not coordinates.
- **Property test.** Generated input drives the app or module and checks a rule that must always hold. Use it when one run shows one path and the bug lives in another.
- **Formal check.** A model checker such as TLA+ checks a state machine in every state, or a proof assistant such as Lean checks an algorithm. Use it when the failure depends on the order of events across parts of a system, or on an edge of an algorithm.

Every proof runs as a mise task, such as `mise run proof:<feature>`, so every agent runs it the same way. If it needs no secrets and runs on a fresh clone, the repo's full check task includes it. If it needs secrets or a deployed environment, it fails closed when they are missing. Follow `dev-tooling-mise`.

A runtime proof, and a property test that drives the app, also meet these rules:

- It drives the real user path, not internal setters or test-only endpoints. It checks the side effects as well as the screen.
- It saves its evidence to a path that it prints, and its cleanup keeps that path.
- The repo's feature map has an entry for the feature: what it is, how a user reaches it, how to drive it, and what end state proves it works. If the repo has no feature map, start one with this feature.

A formal check, or a property test of one module, does not drive the app. It names the rule it checks, and its output shows the rule holding or the case that breaks it.

Fix or delete a test that still passes when the behavior it checks breaks. Also fix or delete a test that fails when only internals change.

## Done when

- The proof fails on the real past failure. Run it on the commit before the fix, or with the fix reverted, and record the output.
- The proof passes on the fix.
- A second run passes with no leftover state from the first.
- Record the command, the result, and the commit SHA. For a runtime proof, and for a property test that drives the app, also record the evidence path.

## Report

- The failure and its link.
- The kind of proof, and why a type, lint, or unit test could not catch the failure.
- The failing and passing output.
- What the proof does not cover.
