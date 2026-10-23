---
name: review
description: >
  Review only the current set of changes. Read the diff, use the language
  server for definitions, references, types, and diagnostics when one is
  available, and do not build or run the project. Sort every finding into
  Act On, Consider, Notes, or Dismiss. Use when the user says "review the
  changes", "review my changes", "review this diff", "review the diff",
  or runs /review.
---

# Review

Review the change set and report findings.

## Scope

Each finding is about the change under review. Also report a side effect that change can cause just outside the diff, such as a caller that now breaks. That is the blast radius. Do not review the rest of the project.

Do not build, test, lint, or run the project. Do not edit files unless the user asks.

## Change set

1. Run `git status --porcelain`.
2. When the user gives a target:
   - A pull request number or URL: review `gh pr diff` for that pull request. Do not comment, approve, or post.
   - A commit, range, or branch: review that `git diff`.
   - A path: limit the diff to that path.
3. Dirty tree, and the user gave no target: review `git diff` and `git diff --cached`, and read untracked files from the status output.
4. Clean tree, and the user gave no target: use `origin/main`, then `origin/master`, as the default branch. Review `git diff <merge-base>..HEAD`.

Turn git color off (`-c color.ui=false -c color.diff=false`). Read the whole diff before reporting. If the diff is empty, say there is nothing to review and stop.

## Inspect

Read the surrounding code before you judge a hunk.

Use the host language server when it is already available. In Grok the tool is `lsp`. In Claude Code the tool is `LSP`. Use it for:

- the type and signature of a changed symbol (hover or go to definition)
- callers of a changed symbol (find references), before reporting that a caller breaks
- diagnostics on changed lines

If that tool is missing, or no server is configured for the language, read the changed files and search for callers. Do not install a language server as a substitute.

## Classify

Each finding goes in exactly one bucket.

- **Act On.** Definitely address this before continuing.
- **Consider.** Does not have to be addressed now. It should be considered at some point.
- **Notes.** A nit, or something that stands out and may not have been intentional. It does not need to be addressed.
- **Dismiss.** A finding you formed and then dropped. Include the reason you dropped it. A dropped finding can spark something else worth considering. Do not list lines you never suspected.

Do not invent a finding to fill a bucket.

## Report

Put the buckets in this order: Act On, Consider, Notes, Dismiss. Omit Consider, Notes, and Dismiss when empty. Always include Act On. If Act On is empty, say there is nothing to address before continuing.

For each finding:

- **Where:** `path:line` in the changed file
- **What:** what is wrong
- **Why this bucket:** the reason, when the bucket is not obvious. For Dismiss, the reason you dropped it.
