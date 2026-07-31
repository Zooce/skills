---
name: rubber-duck-review
description: Perform interactive live rubber-duck reviews of code changes by walking through each hunk or logical change one at a time with explanations, pausing for user feedback before the next, then addressing every comment plus agent observations in iterative fix cycles until the user is happy. Triggers include rubber duck this, live review the diff, go through the changes with me, interactive code review, walk me through the edits, or any request for a conversational step-by-step review of proposed or existing code changes.
---

# Rubber Duck Review

Interactive, conversational review of code changes. The agent presents one logical unit of change at a time, explains it, waits for the user, collects every reaction, then closes the loop by addressing everything before the next full pass.

## When to Activate

Activate immediately when the user asks for a rubber-duck review, live walkthrough, interactive review, or "go through the changes with me". Also offer it proactively when you have just produced a non-trivial set of edits and the user has not yet accepted them.

## Core Loop

1. **Gather the change set**
   - Prefer a clean unified diff (git, proposed patches, or the exact edits you are about to make).
   - If no diff exists yet, produce the intended changes first, then review them.
   - Split into the smallest coherent units that still make sense in isolation (usually one hunk, one function, or one tightly related group of lines). Never dump the entire diff at once.

2. **Present one unit at a time**
   - Show the relevant before/after (or the new code) clearly.
   - Give a short, precise explanation of:
     - what the change does
     - why it is needed
     - any risks, edge cases, or alternative approaches you already notice
   - End with an explicit pause: "Any comments, questions, or changes before we move on?"
   - Do not advance until the user replies.

3. **Record feedback**
   - Capture every user remark (approval, concern, suggested rewrite, question, "skip", etc.).
   - Also record any observations you made while presenting that unit.
   - Keep a running private list; do not lose earlier comments when you move forward.

4. **After the last unit — close the loop**
   - Summarize every collected comment + your own notes in one place.
   - Propose concrete resolutions (code edits, further investigation, or explicit "leave as-is" decisions).
   - Apply the agreed fixes.
   - If the changes are still non-trivial, restart the entire loop on the updated diff.
   - Repeat until the user explicitly signals they are happy ("looks good", "ship it", "approved", etc.).

## Presentation Rules

- Prefer unified-diff style or side-by-side snippets; keep the visible code short.
- Number or label each unit so the user can refer to it later ("Hunk 3", "the error-handling block", etc.).
- When the user asks a clarifying question mid-review, answer it fully, then return to the same unit and re-offer the pause.
- If the user says "skip" or "next", move on but still log that the unit received no substantive feedback.
- Never batch multiple units into one message. One unit → one explanation → one pause.

## Addressing Feedback

- Treat every user comment as a requirement that must be resolved or explicitly deferred with the user's agreement.
- Prefer fixing the code over defending the original version.
- After applying fixes, show the new version of the affected units (or a fresh full pass) rather than claiming "done".
- Keep the conversation moving; do not turn the review into a long design debate unless the user wants one.

## Exit Conditions

End the process only when the user clearly indicates satisfaction with the current state of the changes. A simple "LGTM", "good", or "apply it" is sufficient. Until then, keep cycling.

## Anti-patterns

- Dumping the entire diff or a long list of changes in one message.
- Continuing to the next hunk without an explicit user reply.
- Forgetting earlier comments when later units surface new issues.
- Declaring the review finished without the user saying so.
- Treating the review as a one-way lecture instead of a dialogue.
