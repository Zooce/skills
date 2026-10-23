# Skills

Personal collection of agent skills I use and share. Each skill is a self-contained `SKILL.md` that teaches an AI coding agent a specific workflow.

Compatible with tools that load the [Agent Skills](https://skills.sh/) format (for example Grok, Claude Code, Cursor, and others that honor `SKILL.md`).

## Skills

| Skill | Description |
| ----- | ----------- |
| [design-for-change](./design-for-change/) | Data-first design so code stays easy to change: name input/output shapes before behavior, isolate inseparable work, take only necessary responsibility. |
| [explain-change](./explain-change/) | Plain, high-level explanation of a pull request, diff, or commit range for someone with no context: the problem, what changed by behavior, how it works, what it affects, risks, and verification. |
| [review](./review/) | Review the current change set. Uses a language server when one is available, does not build or run the project, and sorts findings into Act On, Consider, Notes, and Dismiss. |
| [rubber-duck-review](./rubber-duck-review/) | Interactive, conversational review of code changes. Walks through one logical unit at a time, pauses for feedback, then closes the loop with fixes until you are satisfied. |

## Install

With the [Skills CLI](https://skills.sh/):

```bash
# `npx skills` (and awesome tool for this)
npx skills add Zooce/skills

# Install a single skill
npx skills add Zooce/skills@rubber-duck-review

# Or clone and copy into your agent’s skills directory
git clone https://github.com/Zooce/skills.git
cp -r skills/rubber-duck-review ~/.grok/skills/   # Grok (user-wide)
# cp -r skills/rubber-duck-review .grok/skills/    # Grok (project)
```

Exact install paths differ by agent. Check your tool’s docs for where it loads skills from.

## Skill layout

```
skill-name/
  SKILL.md          # Required: frontmatter (name, description) + instructions
  scripts/          # Optional: helper scripts
  references/       # Optional: extra docs the skill may load
```

## License

[MIT](./LICENSE)
