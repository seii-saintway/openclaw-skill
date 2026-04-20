# Channels

25+ messaging platforms supported. All channels share the same DM policy shape.

## Supported Platforms

| Platform | Config key | Notes |
|----------|-----------|-------|
| WhatsApp | `channels.whatsapp` | via Baileys |
| Telegram | `channels.telegram` | bot token required |
| Discord | `channels.discord` | bot token required |
| Slack | `channels.slack` | app + OAuth |
| Google Chat | `channels.googlechat` | |
| Signal | `channels.signal` | |
| iMessage | `channels.imessage` | macOS only (legacy) |
| BlueBubbles | `channels.bluebubbles` | iMessage via BlueBubbles server |
| IRC | `channels.irc` | |
| Microsoft Teams | `channels.msteams` | |
| Matrix | `channels.matrix` | |
| Feishu | `channels.feishu` | |
| LINE | `channels.line` | |
| Mattermost | `channels.mattermost` | |
| Nextcloud Talk | `channels.nextcloud` | |
| Nostr | `channels.nostr` | |
| Synology Chat | `channels.synology` | |
| Tlon | `channels.tlon` | |
| Twitch | `channels.twitch` | |
| Zalo | `channels.zalo` | |
| Zalo Personal | `channels.zalouser` | |
| WeChat | `channels.wechat` | |
| QQ | `channels.qqbot` | |
| WebChat | built-in | http://127.0.0.1:18789 |

## DM Policy

All channels share this pattern:

```json5
{
  channels: {
    telegram: {
      enabled: true,
      botToken: "123:abc",
      dmPolicy: "pairing",       // pairing | allowlist | open | disabled
      allowFrom: ["tg:123456"],  // required for allowlist; use ["*"] for open
    },
  },
}
```

| Policy | Behavior |
|--------|----------|
| `pairing` (default) | Unknown senders get a code; blocked until approved with `openclaw nodes approve` |
| `allowlist` | Only senders in `allowFrom` pass through |
| `open` | All senders allowed; set `allowFrom: ["*"]` to opt in |
| `disabled` | No inbound DMs |

## Pairing Flow

```bash
# User sends a message → receives pairing code
# Approve from CLI:
openclaw pairing approve <channel> <code>
# Or via nodes:
openclaw nodes pending
openclaw nodes approve <id>
```

## Group Messages

Group chats get isolated sessions per group by default. To enable group message handling, configure the channel's group settings. See: https://docs.openclaw.ai/channels/groups

## Channel Routing (multi-agent)

Route different channels to different agents via top-level `bindings`:

```json5
{
  agents: {
    list: [
      { id: "work" },
      { id: "personal" },
    ],
  },
  bindings: [
    { match: { channel: "slack", teamId: "T123" }, agentId: "work" },
    { match: { channel: "telegram" }, agentId: "personal" },
    { match: { channel: "whatsapp" }, agentId: "personal" },
    // Peer-level match (highest priority, overrides account match):
    { match: { channel: "line", peer: { kind: "group", id: "C..." } }, agentId: "work" },
  ],
}
```

Routing priority (highest → lowest):
1. Exact peer match (`peer.kind` + `peer.id`)
2. Parent peer match (thread inheritance)
3. Guild + roles match (Discord)
4. Guild match (Discord)
5. Team match (Slack)
6. Account match (`accountId`)
7. Channel match (any account)
8. Default agent

## Channel-Specific Docs

- WhatsApp: https://docs.openclaw.ai/channels/whatsapp
- Telegram: https://docs.openclaw.ai/channels/telegram
- Discord: https://docs.openclaw.ai/channels/discord
- Slack: https://docs.openclaw.ai/channels/slack
- Signal: https://docs.openclaw.ai/channels/signal
- iMessage/BlueBubbles: https://docs.openclaw.ai/channels/imessage
- Troubleshooting: https://docs.openclaw.ai/channels/troubleshooting
