---
name: qa-verification
description: >
  Prove that a change works the way a user would see it. Write behavior
  tests, reproduce a bug with a minimal failing case, review someone
  else's tests for missing branches, or check a deployed environment.
  Use when proving a change before or after merge, or when the user runs
  /zooce:qa-verification.
---

# QA verification

Report the tested version, exact commands or steps, and observed output or screenshots.

## 1. Reproduce first

- Get a minimal failing case before anyone fixes anything. For a code bug, write a test that fails on the main branch. For a configuration bug with no code-level test, record a failing check on the deployed environment, then record it passing after the fix.
- Record the version under test, such as the commit SHA or the deployed version ID.

## 2. Behavior tests

- Call the code the way its user does: an HTTP request, a CLI command, or a UI action. Assert the exact result the user sees, such as the status, body, exit code, stdout, stderr, files on disk, or calls to stubbed external tools.
- Do not import internals or patch private helpers. A test that breaks on a pure refactor is a defect.
- Do not assert full key sets or full command strings unless that string is the contract.
- Cover these paths:
  - the happy path
  - bad input
  - each guard and early exit
  - an external tool that fails, and one that succeeds with empty or broken output
  - a missing or first-run state
  - a second run: if the operation promises idempotency, it changes nothing. Otherwise, assert the documented repeat behavior.
  - anything deleted upstream

## 3. Review someone else's tests

This is a verification step. It runs the suite and mutations. It is separate from `code-review`, which reads the diff and runs nothing.

1. Check out the head of the change and run the suite with the project's own runner. Record the command, the test count, and the result.
2. Map each branch in the changed code to the test that covers it, with file and line.
3. In a throwaway copy made with `mktemp -d`, remove or invert one guard at a time and rerun the suite. A guard whose removal leaves the suite green is untested.
4. Report in the buckets from `code-review`. Must-fix items and tests tied to internals go in Act On. Nice-to-haves go in Consider. Give file and line for each.

## 4. Checks on a deployed environment

- Start from a signed-out session so the sign-in flow runs.
- For email-code sign-in, use only a code that arrived after your request. If no code arrives within about two minutes, report that as a finding. Do not reuse an old code.
- Capture the browser console, including errors and CSP lines, and the status of the key API calls.
- Treat production as read-only. Use a test account, change nothing except to revert your own test edit, and never delete data.
- Check every state the spec names, and the negative case too.

## 5. Server logs

- Start the log stream before the action you test. Filter for the version, status, and log messages. Report the version you saw.
- Pass the token explicitly on the command. If a valid token gets an authentication error, check that you are using the right account before you decide the token lacks a scope.
- Stop the log stream when you finish.

## Rules

- Never print or store a secret value. Check a secret by its name only.
- A check you did not run is "not run", not "pass".
