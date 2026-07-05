# ai-bits

A personal collection of [Agent Skills](https://agentskills.io/specification) and other AI bits I write — reusable instructions that teach an AI agent (Claude Code, Copilot CLI, Codex, etc.) how to perform a specific task well.

## What's a skill?

A skill is a folder containing a `SKILL.md` file with YAML frontmatter and Markdown instructions. Agents discover a skill by its `description`, then load the body when the triggering conditions match.

```
skills/
  <skill-name>/
    SKILL.md           # required — frontmatter + instructions
    <supporting-file>  # optional — scripts, references, templates
```

The frontmatter has two required fields:

```yaml
---
name: skill-name              # letters, numbers, hyphens only
description: Use when ...      # triggering conditions only (third person)
---
```

> **Convention:** the `description` should describe *when* to use the skill (start with "Use when…"), not *what* it does. Summarizing the workflow in the description tends to make agents follow the summary instead of reading the full skill.

## Skills

| Skill | Use when |
|-------|----------|
| [`quick`](skills/quick/SKILL.md) | The user wants a fast, scannable answer — a recap, TL;DR, or anything starting with `/quick`. |
| [`md-to-plan`](skills/md-to-plan/SKILL.md) | Turn the current markdown file into a structured implementation plan, or invokes `/md-to-plan`. |
| [`sync-skills`](skills/sync-skills/SKILL.md) | Sync skills between this repo and the local agent skills directory (pull down or push up), or invokes `/sync-skills`. |

## Adding a skill

1. Create `skills/<skill-name>/SKILL.md`.
2. Write frontmatter with a `name` and a "Use when…" `description`.
3. Document the behavior, with a Quick Reference and at least one concrete example.
4. Test it: have an agent try to apply it to a realistic scenario before relying on it.
5. Add a row to the table above.

## Using these skills

Drop a skill folder into your agent's skills directory (e.g. `~/.claude/skills/` for Claude Code), or point your plugin/marketplace config at this repo.

## License

[MIT](LICENSE)
