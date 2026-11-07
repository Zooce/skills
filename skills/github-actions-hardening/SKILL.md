---
name: github-actions-hardening
description: >
  Audit and harden GitHub Actions workflows: action pinning, GITHUB_TOKEN
  permissions, pull_request_target, script injection, OIDC, and
  Dependabot. Use when the user asks to audit, harden, or secure a
  workflow, or runs /zooce:github-actions-hardening.
---

# GitHub Actions hardening

1. List every file in `.github/workflows/`. For each one, record the triggers, the `uses:` refs, `permissions:`, secrets, and runners.
2. Pin every `uses:` to a full 40-character commit SHA from the action's own repo. Put `# vX.Y.Z` on the same line so Dependabot can update it. This version comment is the one exception to the no-comments rule in `principles`.
3. Enable Dependabot version updates with `package-ecosystem: github-actions`; alerts do not cover SHA-pinned action updates.
4. Set `permissions: contents: read` (or `{}`) at the top level. Grant write permissions per job only. Ask the repo owner to set the default token to read-only and to turn off "Allow GitHub Actions to create and approve pull requests".
5. Set `persist-credentials: false` on checkout, unless a later step pushes.
6. Avoid `pull_request_target` and `workflow_run` unless the job needs privileged context. In those triggers, never check out or run code from the PR head. Treat `workflow_run` artifacts as untrusted.
7. Never put `${{ github.event.* }}` inside `run:`. Pass the value through `env:` and quote it as `"$VAR"`.
8. For cloud credentials, use OIDC instead of long-lived secrets. Grant `id-token: write` only on the job that needs it, and add conditions on the subject claim.
9. Set `timeout-minutes` on every job. Use `concurrency` with `cancel-in-progress: ${{ github.event_name == 'pull_request' }}` so that runs on the default branch are never cancelled.
10. Add CODEOWNERS for `.github/workflows/`, CodeQL for the `actions` language, and dependency review on pull requests. Do not use self-hosted runners on public repos.
11. Ship each fix as its own small pull request. Check that the names of required checks did not change.

## Sources

- [Secure use reference](https://docs.github.com/en/actions/reference/security/secure-use)
- [OpenID Connect](https://docs.github.com/en/actions/concepts/security/openid-connect)
- [Preventing pwn requests](https://securitylab.github.com/resources/github-actions-preventing-pwn-requests/)
- [Keeping your actions up to date with Dependabot](https://docs.github.com/en/code-security/dependabot/working-with-dependabot/keeping-your-actions-up-to-date-with-dependabot)
