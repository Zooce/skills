---
name: principles
description: >
  Principles for scope, proof, code, and writing. Make a small safe
  slice, prove the work, design from the data with no code comments, and
  keep writing short without dropping meaning. Use when changing code,
  writing, designing a change, reviewing, splitting work into slices, or
  when the user runs /zooce:principles.
---

# Principles

Use these on code and on writing.

## Scope

Make a small, focused change. Do not break the code.

A finished change does not have to affect the system yet. Cut the problem into slices that can be finished that way. That is the path to trunk-based development.

Solve a problem that exists now. Do not solve a problem that might exist later.

## Proof

Prove the work every time.

For code, run a test or the build, or show a screenshot of the result. Show what that check produced.

For writing, a sentence is a fact with evidence, or a decision based on evidence. The evidence is something you ran, read, or were told. Do not state a guess as a fact.

Remove every unknown you can. Name any unknown that remains.

## Code

Do not write a code comment. If the code needs a comment, it is not obvious. Design it again.

Start from the data. Software is data and transformations of that data. A software problem is a data problem.

Code that must change together lives in one place. A caller depends on the result, not on the steps that produce it.

If a caller needs its own copy or its own branch, the shared data is wrong. Change the data. Remove the copy.

Do not invent a problem for a solution. Use a design pattern only when you have the problem it solves now.

## Writing

Prefer a diagram or an image to text.

Keep the writing short and to the point. Trim it, and apply the unslop skill. If a cut would drop a meaning, keep the meaning.
