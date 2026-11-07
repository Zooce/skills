---
name: github-rulesets-required-checks
description: >
  Read, change, or debug repository rulesets, required status checks,
  check-name matching, and bypass actors. Use when a pull request is
  stuck on "Expected, waiting for status", when changing a ruleset, or
  when the user runs /zooce:github-rulesets-required-checks.
---

# Rulesets and required checks

1. Read the rulesets with `gh api repos/OWNER/REPO/rulesets`, then `gh api repos/OWNER/REPO/rulesets/ID`. Read the rules in effect on a branch with `gh api repos/OWNER/REPO/rules/branches/BRANCH`. Some fields need admin access.
2. Required checks match the job's `name:`, or its id if unnamed, not the workflow name. Matrix values become part of the check name, for example `job (a, b)`.
3. These changes leave the check on "Expected" and block every merge:
   - renaming a required job
   - adding a matrix to it
   - a path filter that skips the workflow
4. Before you merge a job rename, get the ruleset updated in the same window. List the required job names in the pull request body.
5. `integration_id` pins a check to one app. GitHub Actions is `15368`.
6. `strict_required_status_checks_policy: true` means the branch must be up to date with the base branch.
7. `bypass_actors` lists the roles, teams, and apps that can skip the rules. Do not add bots. Fix the check instead.
8. Propose ruleset changes to the repo owner. Do not remove protections.

## Sources

- [Available rules for rulesets](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-rulesets/available-rules-for-rulesets)
- [Troubleshooting rules](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-rulesets/troubleshooting-rules)
- [Rules REST API](https://docs.github.com/en/rest/repos/rules)
