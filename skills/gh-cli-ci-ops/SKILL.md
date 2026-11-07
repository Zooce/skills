---
name: gh-cli-ci-ops
description: >
  Use the gh CLI to open and inspect pull requests, watch and debug
  workflow runs, read rulesets, and cut releases. Use when a task needs
  gh for pull requests, CI runs, rulesets, or releases, or the user runs
  /zooce:gh-cli-ci-ops.
---

# gh CLI for pull requests and CI

Any command that writes, such as opening, merging, or rerunning, needs authorization from the repo owner for that action. A reviewer's approval alone never authorizes a merge.

1. Check the account with `gh auth status`. Always name the target repo with `-R OWNER/REPO`, and name the base branch. Do not rely on defaults.
2. Pull requests:
   - Open: `gh pr create -R OWNER/REPO --base BASE --head BRANCH --title TITLE --body-file FILE`
   - State: `gh pr view N -R OWNER/REPO --json state,mergeStateStatus,reviewDecision,statusCheckRollup`
   - Wait for checks: `gh pr checks N -R OWNER/REPO --watch --required`
   - Merge only with the owner's authorization and the method required for the target branch. Check which methods the repo enables:
     `gh api repos/OWNER/REPO --jq '{squash: .allow_squash_merge, merge: .allow_merge_commit, rebase: .allow_rebase_merge}'`
     Use `--squash`, `--merge`, or `--rebase` accordingly. Use `--auto` only when requested.
3. Workflow runs:
   - List: `gh run list -R OWNER/REPO -b BRANCH -L 20 --json databaseId,status,conclusion,name,createdAt`
   - Watch: `gh run watch ID -R OWNER/REPO --exit-status`
   - Failed logs: `gh run view ID -R OWNER/REPO --log-failed`
   - Rerun failed jobs: `gh run rerun ID -R OWNER/REPO --failed`
   - Start: `gh workflow run FILE.yml -R OWNER/REPO -r BRANCH`. The workflow needs a `workflow_dispatch` trigger.
4. Rulesets: read them with `gh ruleset list`, `gh ruleset view ID`, and `gh ruleset check BRANCH`. Do not change rulesets or branch protection. Propose a change to the repo owner, who applies it. A proposal never weakens a protection.
5. Releases: `gh release create vX.Y.Z -R OWNER/REPO --generate-notes`, with `--draft` or `--verify-tag` as needed. Publishing a release is public. Get explicit approval for each release first.
6. In scripts, use `--json` with `--jq`. gh and API timestamps are in UTC. Convert them before you report them.

## Sources

- [gh pr](https://cli.github.com/manual/gh_pr)
- [gh run](https://cli.github.com/manual/gh_run)
- [gh ruleset](https://cli.github.com/manual/gh_ruleset)
- [gh release](https://cli.github.com/manual/gh_release)
