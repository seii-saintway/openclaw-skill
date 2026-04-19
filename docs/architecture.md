# Architecture

## Overview

- A single long-lived **Gateway** owns all messaging surfaces and exposes a WebSocket API.
- Control-plane clients (macOS app, CLI, web UI, automations) connect over WebSocket on `127.0.0.1:18789`.
- **Nodes** (macOS/iOS/Android/headless) connect to the same WS server with `role: node`.
- One Gateway per host; it is the only place that opens a WhatsApp/Telegram/etc. session.
- **Canvas** is served by the Gateway HTTP server at `/__openclaw__/canvas/` and `/__openclaw__/a2ui/`.

## Components

| Component | Role |
|-----------|------|
| **Gateway** | Central daemon — provider connections, WS API, session management, routing, cron |
| **Clients** | CLI, macOS app, web UI — connect via WS, send requests + subscribe to events |
| **Nodes** | Devices (iOS/Android/macOS) — connect with `role: node`, expose device commands |
| **WebChat** | Static UI served by Gateway HTTP, uses the same WS API |
| **Canvas / A2UI** | Agent-driven visual workspace, served at `/__openclaw__/canvas/` |

## WebSocket Protocol

Transport: WebSocket, text frames with JSON payloads.

First frame **must** be `connect`:
```json
{ "type": "req", "id": "1", "method": "connect", "params": { "auth": { "token": "..." } } }
```

After handshake:
- **Requests**: `{ "type": "req", "id", "method", "params" }` → `{ "type": "res", "id", "ok", "payload" | "error" }`
- **Events**: `{ "type": "event", "event", "payload", "seq?", "stateVersion?" }`

Key events: `agent`, `chat`, `presence`, `health`, `heartbeat`, `cron`, `tick`, `shutdown`

Idempotency keys required for side-effecting methods (`send`, `agent`) — the server keeps a short-lived dedupe cache.

Auth modes:
- Shared secret: `connect.params.auth.token` or `.password`
- Tailscale Serve: `gateway.auth.allowTailscale: true`
- Trusted proxy: `gateway.auth.mode: "trusted-proxy"`
- None (private ingress only): `gateway.auth.mode: "none"`

## Session Model

Sessions are isolated conversation contexts keyed by source:

| Source | Session key |
|--------|-------------|
| Direct messages | `main` (shared, default) |
| Group chats | Isolated per group |
| Cron jobs | Fresh per run |
| Webhooks | Isolated per hook |

Session state:
- Store: `~/.openclaw/agents/<agentId>/sessions/sessions.json`
- Transcripts: `~/.openclaw/agents/<agentId>/sessions/<sessionId>.jsonl`

Session reset:
- Daily reset at 4:00 AM local (default)
- Idle reset: `session.reset.idleMinutes`
- Manual: `/new` or `/reset` in chat

## Connection Lifecycle

```
Client → Gateway: req:connect
Gateway → Client: res (ok) + snapshot (presence + health)
Gateway → Client: event:presence
Gateway → Client: event:tick

Client → Gateway: req:agent { message }
Gateway → Client: res:agent (ack { runId, status:"accepted" })
Gateway → Client: event:agent (streaming)
Gateway → Client: res:agent (final { runId, status, summary })
```

## Key Paths

| Path | Content |
|------|---------|
| `~/.openclaw/openclaw.json` | Gateway config (JSON5) |
| `~/.openclaw/workspace/` | Default agent workspace |
| `~/.openclaw/agents/<id>/sessions/` | Session store + transcripts |
| `~/.openclaw/agents/<id>/agent/auth-profiles.json` | Per-agent auth |
| `~/.openclaw/cron/jobs.json` | Persisted cron jobs |
| `~/.openclaw/credentials/` | Stored credentials |

Full protocol reference: https://docs.openclaw.ai/gateway/protocol
Architecture overview: https://docs.openclaw.ai/concepts/architecture
