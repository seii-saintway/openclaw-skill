# Agents & Multi-Agent

## Agent Runtime

OpenClaw runs LLM-backed agents. Each agent has:
- **Workspace** — files, bootstrap docs, local notes, skills
- **State directory** (`agentDir`) — auth profiles, model registry, per-agent config
- **Session store** — chat history + routing state

Default single-agent paths:
- `agentId`: `main`
- Workspace: `~/.openclaw/workspace`
- State: `~/.openclaw/agents/main/agent`
- Sessions: `~/.openclaw/agents/main/sessions`

## Bootstrap Files (injected at session start)

Live in the agent workspace. Blank files are skipped; large files are trimmed.

| File | Purpose |
|------|---------|
| `AGENTS.md` | Operating instructions + "memory" |
| `SOUL.md` | Persona, boundaries, tone |
| `TOOLS.md` | Tool notes and conventions |
| `IDENTITY.md` | Agent name, vibe, emoji |
| `USER.md` | User profile + preferred address |
| `BOOTSTRAP.md` | One-time first-run ritual (deleted after completion) |

To disable bootstrap file creation: `{ agent: { skipBootstrap: true } }`

## Multi-Agent Setup

Multiple isolated agents (separate workspace + auth + sessions) in one Gateway:

```bash
openclaw agents add work      # wizard adds agent + bindings
openclaw agents list
```

Config:
```json5
{
  agents: {
    defaults: {
      workspace: "~/.openclaw/workspace",
      skills: ["github"],             // shared baseline
      sandbox: { mode: "non-main" },
    },
    list: [
      { id: "writer" },              // inherits defaults
      {
        id: "work",
        workspace: "~/.openclaw/workspace-work",
        agentDir: "~/.openclaw/agents/work/agent",
      },
    ],
  },
  bindings: [
    { match: { channel: "slack", teamId: "T123" }, agentId: "work" },
  ],
}
```

**Important**: never reuse `agentDir` across agents — causes auth/session collisions. Auth profiles are per-agent and not shared automatically.

## Skill Allowlists (per-agent)

```json5
{
  agents: {
    defaults: { skills: ["github", "weather"] },  // shared baseline
    list: [
      { id: "writer" },                            // inherits defaults
      { id: "docs", skills: ["docs-search"] },     // replaces defaults (not merged)
      { id: "locked-down", skills: [] },           // no skills
    ],
  },
}
```

Omit `agents.list[].skills` to inherit defaults. A non-empty list is the final set — it does not merge with defaults.

## Agent Loop & Tools

Built-in tools always available (subject to tool policy): `read`, `write`, `edit`, `exec`, `process`, `apply_patch`, `browser`, `canvas`, `sessions_list`, `sessions_history`, `sessions_send`.

`TOOLS.md` provides guidance on how you want tools used — it does not control which tools exist.

## Session Tools

```
sessions_list       — list sessions
sessions_history    — bounded sanitized view of session transcripts
sessions_send       — send a message to another session
```

`sessions_history` strips thinking tags, tool-call XML, and other scaffolding before returning.

## Docs

- Agent runtime: https://docs.openclaw.ai/concepts/agent
- Agent workspace: https://docs.openclaw.ai/concepts/agent-workspace
- Multi-agent: https://docs.openclaw.ai/concepts/multi-agent
- Session model: https://docs.openclaw.ai/concepts/session
