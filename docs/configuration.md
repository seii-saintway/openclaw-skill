# Configuration

Config file: `~/.openclaw/openclaw.json` (JSON5 — supports comments and trailing commas).
Hot-reloaded by the Gateway on change. Unknown keys or malformed values cause boot failure.

When config is invalid, only diagnostic commands work: `openclaw doctor`, `openclaw logs`, `openclaw health`, `openclaw status`.

## Editing Config

```bash
openclaw onboard          # full onboarding wizard
openclaw configure        # config wizard
openclaw config get/set/unset/validate/schema
# Or edit ~/.openclaw/openclaw.json directly
# Control UI: http://127.0.0.1:18789 → Config tab
```

## Minimal Config

```json5
// ~/.openclaw/openclaw.json
{
  agents: { defaults: { workspace: "~/.openclaw/workspace" } },
  channels: { whatsapp: { allowFrom: ["+15555550123"] } },
}
```

## Model Config

```json5
{
  agents: {
    defaults: {
      model: {
        primary: "anthropic/claude-sonnet-4-6",
        fallbacks: ["openai/gpt-5.4"],
      },
      // Allowlist + aliases (if set, becomes the only permitted models)
      models: {
        "anthropic/claude-sonnet-4-6": { alias: "Sonnet" },
        "openai/gpt-5.4": { alias: "GPT" },
      },
      // Specialized models for specific tools
      imageModel: { primary: "anthropic/claude-sonnet-4-6" },
      imageGenerationModel: { primary: "fal/flux-1-schnell" },
      videoGenerationModel: { primary: "runway/gen4-turbo" },
      pdfModel: { primary: "anthropic/claude-sonnet-4-6" },
    },
  },
}
```

## Channel Config (all channels share this shape)

```json5
{
  channels: {
    telegram: {
      enabled: true,
      botToken: "123:abc",
      dmPolicy: "pairing",       // pairing | allowlist | open | disabled
      allowFrom: ["tg:123456"],  // only for allowlist / open
    },
    discord: { dmPolicy: "pairing", allowFrom: ["user123"] },
    slack:   { dmPolicy: "pairing", allowFrom: ["user123"] },
    whatsapp: { allowFrom: ["+15555550123"] },
    signal:   { allowFrom: ["+15555550123"] },
  },
}
```

## Multi-Agent Routing

```json5
{
  agents: {
    defaults: {
      workspace: "~/.openclaw/workspace",
      skills: ["github", "weather"],        // shared baseline skills
      sandbox: { mode: "non-main" },        // Docker sandbox for non-main sessions
    },
    list: [
      { id: "writer" },                             // inherits defaults
      { id: "docs", skills: ["docs-search"] },      // replaces defaults skills
      { id: "locked-down", skills: [] },            // no skills
      {
        id: "work",
        workspace: "~/.openclaw/workspace-work",    // isolated workspace
        agentDir: "~/.openclaw/agents/work/agent",  // isolated auth/state
      },
    ],
  },
}
```

## Session Isolation (multi-user)

```json5
{
  session: {
    dmScope: "per-channel-peer",
    // main (default) | per-peer | per-channel-peer | per-account-channel-peer
  },
}
```

## Sandbox Config

```json5
{
  agents: {
    defaults: {
      sandbox: {
        mode: "non-main",   // off | non-main | all
        scope: "agent",     // agent | session | shared
        backend: "docker",  // docker | ssh | openshell
        docker: {
          binds: ["/data:/data:ro"],
        },
      },
    },
  },
}
```

## Auto-Updater

```json5
{
  updater: {
    enabled: true,
    channel: "stable",  // stable | beta | dev
  },
}
```

## Heartbeat (scheduled agent wakeup)

```json5
{
  agents: {
    defaults: {
      heartbeat: {
        every: "2h",             // interval
        session: "main",
        systemEvent: "Heartbeat: review tasks and send a brief status update.",
      },
    },
  },
}
```

## Key Paths

| Path | Purpose |
|------|---------|
| `~/.openclaw/openclaw.json` | Main config (JSON5, hot-reloaded) |
| `~/.openclaw/credentials/` | Stored credentials |
| `~/.openclaw/workspace/` | Default agent workspace |
| `~/.openclaw/agents/<id>/sessions/` | Session transcripts + store |
| `~/.openclaw/agents/<id>/agent/auth-profiles.json` | Per-agent auth profiles |
| `~/.openclaw/cron/jobs.json` | Persisted cron jobs |

Full reference: https://docs.openclaw.ai/gateway/configuration-reference
Examples: https://docs.openclaw.ai/gateway/configuration-examples
