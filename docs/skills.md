# Skills

Skills teach the agent how and when to use tools. Each skill is a directory containing `SKILL.md` with YAML frontmatter and markdown instructions.

## Loading & Precedence

OpenClaw loads skills from these locations (highest precedence first):

1. `<workspace>/skills` — per-agent workspace skills
2. `<workspace>/.agents/skills` — project agent skills
3. `~/.agents/skills` — personal agent skills (all workspaces)
4. `~/.openclaw/skills` — managed/local skills (all agents)
5. Bundled skills (shipped with install)
6. `skills.load.extraDirs` — extra shared folders (lowest)

Same-name skill: highest-precedence location wins.

## Installing from ClawHub

```bash
openclaw skills search <query>      # browse ClawHub registry
openclaw skills install <slug>      # installs into <workspace>/skills/
openclaw skills update <skill>
openclaw skills update --all
openclaw skills list
openclaw skills info <skill>
openclaw skills check               # validate loaded skills
```

Registry: https://clawhub.ai

## Creating a Custom Skill

```bash
mkdir -p ~/.openclaw/workspace/skills/my-skill
```

`~/.openclaw/workspace/skills/my-skill/SKILL.md`:
```markdown
---
name: my_skill
description: What this skill does and when to use it.
---

# My Skill

Instructions for the agent on how and when to use this skill...
```

Reload: type `/new` in chat or run `openclaw gateway restart`.

## Plugin Skills

Plugins can ship their own skills by listing `skills` directories in `openclaw.plugin.json`. Plugin skills load at the same low precedence as `skills.load.extraDirs`.

## Skill Gating (per-agent)

Control which skills each agent can use:

```json5
{
  agents: {
    defaults: { skills: ["github", "weather"] },
    list: [
      { id: "writer" },                          // inherits defaults
      { id: "docs", skills: ["docs-search"] },   // replacement (not merged)
      { id: "locked-down", skills: [] },         // no skills
    ],
  },
}
```

## Security

Treat third-party skills as **untrusted code**. Read them before enabling. Workspace and extra-dir skill discovery only accepts skill roots whose resolved realpath stays inside the configured root.

Docs: https://docs.openclaw.ai/tools/skills
Creating skills: https://docs.openclaw.ai/tools/creating-skills
ClawHub: https://docs.openclaw.ai/tools/clawhub
