---
name: explain-change
description: >
  Explain a code change to someone with no context: what problem it solves, what changed in plain words, how it works at a high level, what it affects, and what to watch. Use for a pull request, diff, commit, commit range, or branch. Triggers include explain this change, explain this PR like I'm new, what does this diff do, catch me up on this branch, summarize these commits, or /explain-change.
---

# Explain Change

Explain a change so a newcomer understands it. No assumed context, no jargon, no invented facts.

## When to Activate

Activate when the user asks what a pull request, diff, commit, commit range, or branch does, or asks to be caught up on a change. Also offer it when handing a change to someone who was not involved in it.

## Gather Context

1. **The change itself.** Get the full diff (for example `git diff base...head`, `git show`, or the pull request's diff). Read all of it before explaining any of it.
2. **The stated intent.** Read the pull request title and description, commit messages, and any linked issues or tickets.
3. **The surrounding code.** Read enough of the code around each change to know what it is part of and who calls it.
4. **The evidence.** Note any tests added or changed, CI results, and verification described in the pull request.

If you cannot get one of these, say so in the explanation instead of guessing.

## Output

Use these sections, in this order. Skip a section only when there is nothing to say, and say that.

1. **The problem.** What was wrong or missing, and why it is being fixed now. One or two sentences.
2. **What changed.** Plain words, grouped by behavior the user or caller would notice, not by file. "Sign-in now retries once on timeout," not "Updated auth.ts."
3. **How it works.** The idea behind the change at a high level: the main data and the steps that transform it. No line-by-line walkthrough.
4. **What it affects.** Who or what sees a difference: users, callers, other services, data, configuration, deployments.
5. **Risks and things to watch.** What could break, edge cases, migrations, follow-up work, anything reversible only with effort.
6. **How it was verified.** Tests, CI runs, or manual checks, if known. If nothing shows it was verified, say so.
7. **Open questions.** Anything the change does not make clear.

## Rules

- Define every technical term in one short line the first time you use it, or use a plainer word.
- Never invent intent, history, or facts. If the change does not say why, write "the change doesn't say why."
- Separate what the change shows from what you infer. Mark inferences as such.
- Keep it short: a reader should finish in about two minutes. Offer more depth on any section on request instead of adding it up front.
- Prefer concrete examples over abstractions: one real input and its new output beats a paragraph.

## Anti-patterns

- Walking through the diff file by file.
- Restating code as prose ("this line sets x to 5").
- Explaining with terms the reader would have to look up.
- Filling gaps with plausible guesses about intent.
- Long explanations nobody asked for.
