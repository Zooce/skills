---
name: code-review
description: >
  Review the code in the current change set. Use a language server when
  one is available. Do not build or run the project. Docs and other
  writing are writing-review. Use when the user says "review the
  changes", "review my changes", "review this diff", "review the diff",
  or runs /zooce:code-review.
---

# Code review

Review the code in the change and report findings.

Review source, tests, and config that changes how the program builds or runs. Docs and other writing are writing-review. A comment or string is in scope only when it disagrees with the code.

Stay on the change. Also report a side effect just outside it, such as a caller that now breaks. Do not review the rest of the project. Do not build, test, lint, or run it. Do not edit unless asked.

Read the surrounding code before you judge a change. When a language server is already available, use it for the type of a changed symbol, its callers, and diagnostics on the changed lines. If there is none, read the code and search for callers.

Put each finding in one bucket.

- **Act On.** Address this before continuing.
- **Consider.** It can wait, and it should be decided later.
- **Notes.** A nit, or something that may not have been intentional.
- **Dismiss.** A finding you formed and then dropped. Say why.

Report Act On, then the others. Omit an empty bucket. Always include Act On. If nothing needs to be addressed, say so. For each finding, give the place and what is wrong.
