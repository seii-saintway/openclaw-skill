# Platforms & Apps

All apps are optional. The Gateway alone delivers a complete experience.

## macOS App (OpenClaw.app)

- Menu bar control for Gateway health and status
- Voice Wake (wake words) + push-to-talk overlay
- Live Canvas + A2UI visual workspace
- WebChat and debug tools
- Remote gateway control over SSH

**Signed builds required** for macOS permissions to persist across rebuilds.
Permissions guide: https://docs.openclaw.ai/platforms/mac/permissions

Canvas is served at `/__openclaw__/canvas/` (same port as Gateway, default 18789).
A2UI is served at `/__openclaw__/a2ui/`.

## iOS Node

- Pairs as a node over Gateway WebSocket (device pairing)
- Voice trigger forwarding + Canvas surface
- Controlled via `openclaw nodes ...`

Runbook: https://docs.openclaw.ai/platforms/ios

## Android Node

- Pairs via device pairing (`openclaw devices ...`)
- Exposes Connect/Chat/Voice tabs
- Camera, Screen capture, Android device command families
- Canvas surface

Runbook: https://docs.openclaw.ai/platforms/android

## Voice Wake & Talk Mode

- **Voice Wake** — wake words on macOS/iOS
- **Talk Mode** — continuous voice on Android
- TTS: ElevenLabs + system TTS fallback

Node voice docs: https://docs.openclaw.ai/nodes/talk
Voice Wake: https://docs.openclaw.ai/nodes/voicewake

## Canvas & A2UI

Agent-driven visual workspace:
- Canvas: agent-editable HTML/CSS/JS at `/__openclaw__/canvas/`
- A2UI: structured UI components at `/__openclaw__/a2ui/`

Canvas docs: https://docs.openclaw.ai/platforms/mac/canvas

## Node Commands

Nodes expose invoke commands:

| Command family | Description |
|---------------|-------------|
| `canvas.*` | Canvas rendering and interaction |
| `camera.*` | Camera capture |
| `screen.record` | Screen recording |
| `location.get` | GPS/location |
| `audio.*` | Audio playback/recording |

## Remote Access

- Tailscale: https://docs.openclaw.ai/gateway/tailscale
- SSH tunnel: https://docs.openclaw.ai/platforms/mac/remote
- VPS deployment: https://docs.openclaw.ai/vps

## Other Platforms

- Linux: https://docs.openclaw.ai/platforms/linux
- Windows (WSL2 recommended): https://docs.openclaw.ai/platforms/windows
- Raspberry Pi: https://docs.openclaw.ai/platforms/raspberry-pi
- Docker: https://docs.openclaw.ai/install/docker
- Kubernetes: https://docs.openclaw.ai/install/kubernetes
- Nix: https://docs.openclaw.ai/install/nix
