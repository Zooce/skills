---
name: design-it-out
description: >
  Change the code so a failure cannot be written again. Remove the
  choice that allowed it with a data shape, an API, or a boundary, and
  delete every old example an agent could copy. Use when a bug, a
  revert, or a repeated review correction comes from a choice the code
  should not offer, or when the user runs /zooce:design-it-out.
---

# Design it out

Design for an agent that sees only the files it opened, copies the nearest example, and takes the shortest path that compiles. A choice that the code does not offer needs no check, no proof, no rule, and no reviewer.

## Input

One failure with a link: a bug, a revert, an incident, or a review comment that keeps coming back. If there is no link, say so.

## Name the choice that allowed it

Say which choice the code gave its author and how that choice led to the failure. Common choices:

- Two copies of the same data, or two lists, that have to be kept in sync and then drift apart.
- A data shape that can hold a state the product never wants, such as a loading flag and an error set at the same time.
- An API that takes judgment to use well, offered everywhere.
- An import across a module boundary that the design forbids.
- A workaround that has been copied until it looks like the pattern.

Start from the data, as `principles` says. If a caller needs its own copy or its own branch, the shared data is wrong.

## The finished change

The change removes the choice. It does one or more of these:

- Changes the data so the bad state has no representation. Give each piece of state one owner.
- Gives the task one supported way. Make the others private, or delete them.
- Hides internals so a wrong import fails. Prefer a language boundary, such as a private or unexported symbol. Use `catch-it-statically` when the language cannot enforce it.
- Replaces lists that have to be kept in sync by hand with one source of truth.
- Puts code with different rules in directories whose names say the rule. Enforce the split with `catch-it-statically`.
- Fixes the conflict that the workaround hid, then deletes the workaround.

It also removes every copy of the old way: callers, workarounds, deprecated APIs, dead code, and comments that justify them. Agents copy the nearest example, so one copy left behind becomes the pattern again.

If the old way has too many callers for one small change, block new uses with `catch-it-statically` first. Then move the callers in slices and delete the old way with the last one.

## Done when

- The failure from the input cannot be written. Show the attempt and the error it gets, or show that the API or state no longer exists.
- No copy of the old way remains. Show the search command and its empty result.
- The repo's full check passes, such as `mise run verify`. Record the command, the result, and the commit SHA.
- The change adds no code comment.

## Report

- The failure and its link.
- The choice you removed, and how.
- The search that shows no copy remains.
- Anything this change could not stop, and the skill that handles it.
