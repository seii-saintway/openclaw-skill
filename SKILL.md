---
name: openclaw
description: OpenClaw personal AI assistant — install, configure, manage the Gateway, channels, nodes, agents, skills, sessions, cron, security, and development workflows. Use for any CLI command, config pattern, architecture question, or troubleshooting.
---

# OpenClaw Skill

OpenClaw is a self-hosted personal AI assistant you run on your own devices. It answers you on the messaging channels you already use (25+ platforms), supports voice on macOS/iOS/Android, and renders a live Canvas. The Gateway is the control plane — the product is the assistant.

## When to Use This Skill

Use this skill when the user:
- Wants to install, onboard, or update OpenClaw
- Needs help configuring or connecting a channel (WhatsApp, Telegram, Discord, Slack, Signal, iMessage, etc.)
- Asks about any CLI command (gateway, agent, message, pairing, doctor, security, nodes, cron, skills, webhooks, config, models, plugins)
- Wants to understand the architecture (Gateway, Channels, Nodes, Sessions, Agents, Skills, Canvas, A2UI)
- Needs to configure multi-agent routing, DM policy, session isolation, sandbox security, or model failover
- Is setting up automation (cron jobs, webhooks, Gmail Pub/Sub)
- Asks about Voice Wake, Talk Mode, Live Canvas, A2UI, or memory
- Is developing with OpenClaw (building plugins, running from source, writing skills)
- Needs to troubleshoot connection issues or run a security audit
- Wants to configure models, fallbacks, providers, or local models

## ⚡ Quick Reference

```bash
# Install
curl -fsSL https://openclaw.ai/install.sh | bash
openclaw onboard --install-daemon

# Gateway
openclaw gateway status / run / health
openclaw dashboard / tui / status

# Performance (suggested for Pi/VM)
export NODE_COMPILE_CACHE=/var/tmp/openclaw-compile-cache
export OPENCLAW_NO_RESPAWN=1

# Agent & messaging
openclaw agent --target +1234567890 --message "Hello" --thinking high
openclaw message send --target +1234567890 --message "Text"

# Security & maintenance
openclaw security audit [--fix] [--deep]
openclaw doctor [--fix]
openclaw update [--channel stable|beta|dev]

# Sessions & nodes
openclaw sessions
openclaw nodes status / list / approve / reject / rename / describe

# Skills
openclaw skills search / install / update / list / info / check

# Config
openclaw config get/set/unset/validate/schema
openclaw models list / status / set <provider/model>
openclaw agents add <id>
```

Chat commands: `/new [model]`, `/reset`, `/compact`, `/think <level>`, `/verbose on|off`, `/usage off|tokens|full`, `/restart`

## Reference Docs

- [CLI Commands](docs/cli.md) — full command reference for every subcommand
- [Configuration](docs/configuration.md) — config file patterns, examples, hot-reload
- [Channels](docs/channels.md) — setup guide for all 25+ messaging platforms
- [Architecture](docs/architecture.md) — Gateway, Nodes, Sessions, WebSocket protocol
- [Agents & Multi-Agent](docs/agents.md) — agent runtime, workspace, bootstrap files, multi-agent routing
- [Skills](docs/skills.md) — skill loading, precedence, creating custom skills, ClawHub
- [Models & Providers](docs/models.md) — model selection, fallbacks, auth profiles, supported providers
- [Security & Sandbox](docs/security.md) — DM policy, sandboxing modes, security audit
- [Memory](docs/memory.md) — MEMORY.md, daily notes, dreaming, memory-wiki
- [Automation](docs/automation.md) — cron jobs, webhooks, Gmail Pub/Sub, hooks
- [Platforms](docs/platforms.md) — macOS app, iOS node, Android node, Canvas, A2UI
- [Development](docs/development.md) — build from source, dev loop, Docker, plugins SDK

## Key Documentation Links

| Goal | Link |
|------|------|
| Getting started | https://docs.openclaw.ai/start/getting-started |
| Configuration | https://docs.openclaw.ai/gateway/configuration |
| Config reference | https://docs.openclaw.ai/gateway/configuration-reference |
| Channels index | https://docs.openclaw.ai/channels |
| Security | https://docs.openclaw.ai/gateway/security |
| Architecture | https://docs.openclaw.ai/concepts/architecture |
| Skills | https://docs.openclaw.ai/tools/skills |
| ClawHub | https://clawhub.ai |
| Troubleshooting | https://docs.openclaw.ai/channels/troubleshooting |
