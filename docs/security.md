# Security & Sandbox

## DM Policy

All inbound DMs are treated as untrusted input. Configure per-channel:

| Policy | Behavior |
|--------|----------|
| `pairing` (default) | Unknown senders get a code; blocked until approved |
| `allowlist` | Only senders in `allowFrom` pass |
| `open` | All senders; requires `allowFrom: ["*"]` opt-in |
| `disabled` | No inbound DMs |

```json5
{
  channels: {
    telegram: { dmPolicy: "pairing" },
    discord:  { dmPolicy: "pairing", allowFrom: ["user123"] },
  },
}
```

Approve pairing:
```bash
openclaw pairing approve <channel> <code>
openclaw nodes approve <id>
```

## Security Audit

```bash
openclaw security audit           # check config for common foot-guns
openclaw security audit --fix     # apply safe remediations + chmod fixes
openclaw security audit --deep    # include live Gateway probe
openclaw security audit --json    # JSON output
```

Surfaces: risky DM policies, `session.dmScope` issues, permission misconfigs.

## Doctor

```bash
openclaw doctor         # broader diagnostics + legacy checks
openclaw doctor --fix   # apply repairs (alias: --yes)
```

When config is invalid, only doctor/logs/health/status work.

## Sandboxing

Tools run on the host by default. Sandboxing (via Docker/SSH/OpenShell) limits filesystem and process access.

### Sandbox Modes

`agents.defaults.sandbox.mode`:
- `"off"` — no sandboxing
- `"non-main"` — sandbox non-main sessions (group chats, channels)
- `"all"` — sandbox every session

### Sandbox Scope

`agents.defaults.sandbox.scope`:
- `"agent"` (default) — one container per agent
- `"session"` — one container per session
- `"shared"` — one container shared by all sandboxed sessions

### Sandbox Backends

| Backend | Setup | Best for |
|---------|-------|---------|
| `docker` | `scripts/sandbox-setup.sh` | Local isolation |
| `ssh` | SSH key + remote host | Offload to remote machine |
| `openshell` | OpenShell plugin enabled | Managed remote sandboxes |

### Typical Config

```json5
{
  agents: {
    defaults: {
      sandbox: {
        mode: "non-main",
        scope: "agent",
        backend: "docker",
        docker: {
          binds: ["/data:/data:ro"],
        },
        // Browser in sandbox (Docker only)
        browser: {
          autoStart: true,
        },
      },
    },
  },
}
```

### What Gets Sandboxed

- Tool execution: `exec`, `read`, `write`, `edit`, `apply_patch`, `process`
- Optional: sandboxed browser

Not sandboxed:
- The Gateway process itself
- Tools in `tools.elevated` (bypass sandbox, run on host)

### Elevated Mode

`tools.elevated` tools bypass sandboxing and run on the host. Use with caution.

## Remote Exposure

Before exposing the Gateway remotely:
- Read the security guide: https://docs.openclaw.ai/gateway/security
- Use Tailscale or SSH tunnel — do not expose port 18789 directly
- Set `gateway.auth.mode` appropriately
- Review Docker `DOCKER-USER` firewall policy for VPS deployments

Tailscale: https://docs.openclaw.ai/gateway/tailscale
Remote access: https://docs.openclaw.ai/gateway/remote

## Session Isolation (multi-user)

If multiple people can message your agent, enable DM isolation:

```json5
{
  session: {
    dmScope: "per-channel-peer",
    // main | per-peer | per-channel-peer | per-account-channel-peer
  },
}
```

Without this, all DMs share one session — Alice's messages visible to Bob.

Docs: https://docs.openclaw.ai/gateway/security
Sandboxing: https://docs.openclaw.ai/gateway/sandboxing
