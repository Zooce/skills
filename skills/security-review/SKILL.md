---
name: security-review
description: >
  Review a plan or change for security risk, review a change to who
  holds a credential, or handle a possibly leaked secret. Use when the
  user asks for a security review, when a change touches auth, secrets,
  CI, or access, or when the user runs /zooce:security-review.
---

# Security review

Find real risk and get it fixed. Access stays as wide as the work needs and no wider. Do not report nitpicks.

## Report

Read the diff yourself. Do not trust a summary.

Use the buckets from `code-review`. Omit an empty bucket. Always include Act On. Tag each finding with its severity.

- **Act On.** Critical, high, and medium findings. Critical and high block the merge.
- **Consider.** Low findings, each with a named follow-up.
- **Notes.** Context that needs no action.

End with one verdict for the exact commit reviewed: APPROVE, FIX-THEN-APPROVE, or BLOCK. A new commit needs a new review.

## Checklist

1. **Secrets in the diff.** Look for tokens, keys, private keys, JWTs, `.env` files, and logs or screenshots that show a value. Publishable keys and account IDs are not secrets.
2. **Credential targeting.** Each cloud call names its token and target account explicitly. Nothing relies on a token already set in the environment. Production is not reachable from test tooling. Scripts use `set -eu` and stop when a token is missing, the tree is dirty, or the target is wrong.
3. **Destructive operations.** Production migrations and data changes take a backup first and have a revert path.
4. **Auth.** Server-side token checks verify the issuer, the audience or authorized parties (exact origins), and expiry. Production builds have no mock auth mode and no bypass. One user cannot reach another user's data by ID.
5. **Injection and input.** SQL is parameterized. No `innerHTML` or bypassed sanitizer with user data. The server validates input.
6. **Headers and CORS.** CSP is enforced, not only report-only, after it is verified. No wildcard CORS with credentials. HSTS is on.
7. **CI.** Actions are pinned to SHAs. `permissions:` is least privilege. No `pull_request_target` that checks out PR code. No `${{ github.event.* }}` in `run:`. Rulesets are not weakened, and bypass lists stay empty.
8. **Supply chain.** Each new dependency has a reason. The lockfile is updated. Nothing is fetched from an unpinned URL.
9. **Automation.** Pin sync and deploy inputs to full commit SHAs on the main branch. Reject failed or stale reports. Roll back only with the owner's explicit recovery approval.
10. **Rule changes.** A change to rules or workflows does not remove or weaken a security, secrets, or access rule. Say so in the review.

## Secrets

- Never paste, log, print, screenshot, or commit a secret value.
- Keep shared secrets only in the platform's secret store. Use one scoped token per job, with an expiry. Do not create a token that can create other tokens.
- Take no screenshots, accessibility snapshots, or captured page output while a secret value is on screen. On a page that reveals a key, use the provider's copy control and do not display the value. Delete any capture from that session by path, without opening it.
- Save a value from the clipboard to a file without echoing it, for example `read -rs K && (umask 077; printf %s "$K" > FILE) && unset K`.
- To check a secret, use its length, file mode, or prefix. Never print it. Prove it works by the HTTP status code only.
- When you rotate a secret, keep the old one until a call with it fails after revocation. Then delete it.
- Keep a list of credentials with name, scope, holder, location, expiry, revoke path, and status. Never put a value in it. Update it in the same change as any grant, rotation, or revocation.
- The account owner keeps root access, 2FA, billing, branch protection, and the kill switch.

## Leaked secret

1. Stop. Do not display the value again to check it.
2. Revoke or rotate it at the source now. If you cannot, ask the owner to revoke it.
3. Remove copies from terminal history, files, screenshots, and PR text. If it was committed, revoke it first. Rewrite history only if you must.
4. Create the replacement without displaying it. Check that it works and that the old one fails.
5. Record the leak in the credential list with the date, severity, and whether it was contained.
