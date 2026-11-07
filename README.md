# zooce

Plugin for Grok and Claude Code. It only contains skills for now. Each skill is a `SKILL.md`. The marketplace has the same name.

## Skills

| Skill | Description |
| ----- | ----------- |
| [principles](./skills/principles/) | Make a small safe slice, prove the work, design from the data with no code comments, and keep writing short without dropping meaning. |
| [code-review](./skills/code-review/) | Review the code in the current change. Uses a language server when one is available, and does not build or run the project. |
| [writing-review](./skills/writing-review/) | Review docs and other writing. Applies technical-writing. |
| [technical-writing](./skills/technical-writing/) | Writing standard for docs, RFCs, READMEs, pull requests, and commit messages. Copied from pstack (Lauren Tan, MIT). |
| [understand-the-problem](./skills/understand-the-problem/) | Answer what the problem is, including parts you discover, who it affects and why they would care, and whether it is a symptom of a different problem. Look up a goal, a Jira issue, or work in the current project before answering. |
| [unslop](./skills/unslop/) | Remove AI tells from writing. Copied from pstack (Lauren Tan, MIT). `technical-writing` applies this skill. |
| [github-actions-hardening](./skills/github-actions-hardening/) | Audit and harden GitHub Actions workflows: action pinning, token permissions, risky triggers, script injection, and OIDC. |
| [gh-cli-ci-ops](./skills/gh-cli-ci-ops/) | Use the gh CLI for pull requests, workflow runs, rulesets, and releases. |
| [github-rulesets-required-checks](./skills/github-rulesets-required-checks/) | Read, change, and debug rulesets and required status checks, including checks stuck on "Expected". |
| [dev-tooling-mise](./skills/dev-tooling-mise/) | Set up a repo's tools and tasks with mise so a fresh checkout runs and agents can run every task safely. |
| [ui-review-checklist](./skills/ui-review-checklist/) | Review a user-facing change with evidence from the real UI: states, accessibility, and copy. |

## Install

With the [Skills CLI](https://skills.sh/):

```bash
npx skills add Zooce/skills
npx skills add Zooce/skills@code-review
```

The plugin:

```bash
# Grok
grok plugin marketplace add Zooce/skills
grok plugin install zooce --trust

# Claude Code
claude plugin marketplace add Zooce/skills
claude plugin install zooce@zooce
```

## Layout

```
.claude-plugin/
  plugin.json
  marketplace.json
skills/
  skill-name/
    SKILL.md
    scripts/       # optional
    references/    # optional
```

## License

[MIT](./LICENSE)

`unslop` and `technical-writing` are unchanged copies from [pstack](https://github.com/backnotprop/pstack) by Lauren Tan (poteto), taken at commit `37d989cabf310ea2d7631f1d006117ca4cc8a799`. Upstream is [cursor/plugins/pstack](https://github.com/cursor/plugins/tree/main/pstack). Copyright (c) 2026 Lauren Tan. Each skill directory has its own `LICENSE` and `ATTRIBUTION.md`, so the notice stays with the skill when it is copied on its own.
