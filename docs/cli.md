# CLI Commands

Runtime: **Node 24 (recommended) or Node 22.12+**

## Performance Optimization

On small hosts (Pi, VM, etc.), set these environment variables for faster startup:

```bash
export NODE_COMPILE_CACHE=/var/tmp/openclaw-compile-cache
mkdir -p /var/tmp/openclaw-compile-cache
export OPENCLAW_NO_RESPAWN=1
```

## Install & Onboard

```bash
# macOS / Linux
curl -fsSL https://openclaw.ai/install.sh | bash

# Windows (PowerShell)
iwr -useb https://openclaw.ai/install.ps1 | iex

# npm / pnpm / bun
npm install -g openclaw@latest
pnpm add -g openclaw@latest
bun add -g openclaw@latest

openclaw onboard --install-daemon   # interactive setup + launchd/systemd service
openclaw configure                  # config wizard (post-install)
openclaw setup                      # create config + init workspace files
```

## Gateway

```bash
openclaw gateway status             # check if running (via Gateway RPC)
openclaw gateway run                # run in foreground (WebSocket Gateway)
openclaw gateway run --port 18789 --verbose
openclaw gateway health             # health probe
openclaw gateway call <method>      # raw RPC call
openclaw gateway usage-cost         # usage cost summary
openclaw gateway discover           # discover gateways on the local network
openclaw dashboard                  # open web Control UI (http://127.0.0.1:18789)
openclaw tui                        # terminal UI
openclaw status                     # top-level status (routed)
openclaw health                     # top-level health check
openclaw logs                       # view gateway logs
```

## Agent & Messaging

```bash
openclaw agent --message "Hello" --thinking high
openclaw message send --to +1234567890 --message "Text"
openclaw pairing approve <channel> <code>   # approve DM pairing request
openclaw pairing list
```

## Security & Maintenance

```bash
openclaw security audit             # audit config for common foot-guns
openclaw security audit --fix       # apply safe remediations + chmod fixes
openclaw security audit --deep      # include live Gateway probe
openclaw security audit --json      # JSON output
openclaw doctor                     # broader diagnostics + legacy checks
openclaw doctor --fix               # apply doctor repairs (alias: --yes)
openclaw update                     # update (detects npm or git install)
openclaw update --channel stable|beta|dev
openclaw update --tag main          # raw npm dist-tag
openclaw update --dry-run
openclaw backup                     # backup config + state
```

## Sessions & Nodes

```bash
openclaw sessions                   # list active sessions
openclaw nodes status               # list known nodes with connection status
openclaw nodes list                 # list pending + paired nodes
openclaw nodes pending              # list pending pairing requests
openclaw nodes approve <id>
openclaw nodes reject <id>
openclaw nodes rename <id> <name>
openclaw nodes describe <id>        # capabilities + supported invoke commands
openclaw devices                    # Android device pairing
```

## Cron Jobs

```bash
openclaw cron status                # scheduler status
openclaw cron list
openclaw cron add \
  --name "Daily brief" \
  --cron "0 9 * * *" \
  --tz America/New_York \
  --session main \
  --system-event "Send morning brief"
openclaw cron add \
  --name "Reminder" \
  --at "20m" \                      # relative time
  --delete-after-run                # one-shot
openclaw cron rm <id>
openclaw cron runs --id <id>        # run history
openclaw cron run <id>              # run now (debug)
```

Schedule types: `--at <ISO8601 or relative>`, `--every <interval>`, `--cron <5/6-field expr> [--tz]`

## Skills

```bash
openclaw skills search <query>      # search ClawHub
openclaw skills install <slug>      # install into workspace skills/
openclaw skills update <skill>
openclaw skills update --all
openclaw skills list
openclaw skills info <skill>
openclaw skills check               # validate installed skills
```

## Models

```bash
openclaw models list
openclaw models status
openclaw models set <provider/model>
openclaw models scan                # probe available models
```

## Multi-Agent

```bash
openclaw agents add <id>            # add isolated agent via wizard
openclaw agents list
```

## Config

```bash
openclaw config get agents.defaults.workspace
openclaw config set agents.defaults.heartbeat.every "2h"
openclaw config unset plugins.entries.brave.config.webSearch.apiKey
openclaw config validate
openclaw config schema              # full JSON Schema
```

## Plugins

```bash
openclaw plugins install <plugin>
openclaw plugins update <plugin>
openclaw plugins list
openclaw plugins uninstall <plugin>
```

## Webhooks

```bash
openclaw webhooks gmail setup       # Gmail watch + Pub/Sub + hooks
openclaw webhooks gmail run         # run gog watch serve + auto-renew
```

## Other

```bash
openclaw secrets list / set / unset
openclaw channels add               # add a new channel
openclaw directory                  # manage directory entries
openclaw qr                         # show QR code for pairing
openclaw dns                        # DNS helpers
openclaw completion                 # shell completion setup
openclaw uninstall                  # uninstall OpenClaw
```

## Chat Commands (in-channel)

| Command | Description |
|---------|-------------|
| `/new [model]` | Start a new session, optionally switch model |
| `/reset` | Reset current session |
| `/compact` | Compact session context |
| `/think <level>` | Set thinking level |
| `/verbose on\|off` | Toggle verbose output |
| `/trace on\|off` | Toggle trace output |
| `/usage off\|tokens\|full` | Usage reporting mode |
| `/restart` | Restart the gateway |
| `/activation mention\|always` | Activation mode |
| `/status` | Show gateway status |
