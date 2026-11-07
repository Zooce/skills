---
name: dev-tooling-mise
description: >
  Set up a repo's tools and tasks with mise so that a fresh checkout
  runs with documented commands and agents can run every task safely.
  Use when adding or changing mise.toml, task scripts, local setup docs,
  or a deploy, migrate, or sync command, or when the user runs
  /zooce:dev-tooling-mise.
---

# Dev tooling with mise

## Setup

1. The `[tools]` pins in `mise.toml` are the source of truth. Keep `.nvmrc`, `engines`, and `.python-version` the same. Commit `mise.lock`.
2. Tasks call the existing package scripts, such as `npm run build`. Do not copy their logic into mise.
3. The README gives the steps in this order: install mise, `mise trust`, `mise install`, `mise run <task>`. CI and agents run the same tasks.
4. Local defaults, such as mock auth and a local database, work with no secrets.

## Tasks

- Start each multi-line `run` with `set -eu`. Add `umask 077` when the task writes backups.
- `quiet = true` hides the mise header. Keep machine-readable output, such as JSON, alone on stdout. Send diffs and logs to stderr.
- Pass arguments with `mise run TASK -- --flag`.
- Fail closed. Check required environment variables and stop before any side effect. Refuse a dirty tree with `[ -z "$(git status --porcelain)" ]`. `git diff --quiet` misses untracked files.
- Tag each deploy with the git commit SHA.
- Prove each task before you ship it. Run it for real where that is safe. Also run the failure cases, such as an unset token, a dirty tree, and bad input. Each must exit nonzero before any side effect.

## Secrets

- Never print, log, or commit a token. Never store one in agent memory or notes.
- Keep tokens only in a sanctioned store: the platform's secret store, CI secrets, or a local secrets file with mode 600 in a directory with mode 700.
- Pass each token on the call that uses it, for example `API_TOKEN="$STAGE_TOKEN" tool deploy`. Never rely on a token that is already set in the environment.
- Before a production task runs, check that the target account is correct. Back up first and check that the backup is not empty. Then migrate.

## Gotchas

- `mise run` puts the pinned binary first on `PATH`, ahead of any test stub. To test with a stub, run the task script directly.
- Frontend build variables must be set at build time. Runtime variables never reach the frontend bundle.
- Put build guards in the deploy tool's own build hook, so every deploy path runs them. A guard that only lives in a task can be skipped.
- A bundle guard matches exact expressions and quoted strings in every output chunk, not bare words. Remove each check in turn and confirm that a test fails.
- A deploy to a shared test environment replaces other people's builds. Coordinate with anyone using the shared environment before deploying. Migrations from an unmerged branch are permanent.
- Use `mktemp -d` for temporary directories. A fixed path such as `/tmp/build` can collide with other processes.
