# Development

## Build from Source

```bash
git clone https://github.com/openclaw/openclaw.git
cd openclaw

pnpm install
pnpm ui:build          # auto-installs UI deps on first run
pnpm build             # outputs dist/
pnpm openclaw onboard --install-daemon

# Dev loop (auto-reload on source/config changes)
pnpm gateway:watch

# Run TypeScript directly (no build step, via tsx)
pnpm openclaw <command>

# macOS unified logs
./scripts/clawlog.sh

# Docker (optional containerized gateway)
./scripts/docker/setup.sh
```

`pnpm openclaw ...` runs TypeScript directly via `tsx`. `pnpm build` produces `dist/` for Node / the packaged `openclaw` binary.

## Development Channels

| Channel | Tag format | npm dist-tag | Notes |
|---------|-----------|--------------|-------|
| stable | `vYYYY.M.D` | `latest` | Tagged releases |
| beta | `vYYYY.M.D-beta.N` | `beta` | Prerelease; macOS app may be missing |
| dev | moving `main` head | `dev` | When published |

Switch: `openclaw update --channel stable|beta|dev`
One-off tag: `openclaw update --tag main`

Dev channels: https://docs.openclaw.ai/install/development-channels

## Plugin SDK

Plugins can add channels, providers, tools, skills, and agent harnesses.

Plugin manifest (`openclaw.plugin.json`):
```json
{
  "name": "my-plugin",
  "version": "1.0.0",
  "skills": ["./skills"],
  "entrypoints": {
    "gateway": "./dist/gateway.js"
  }
}
```

Plugin can register:
- Channel plugins — new messaging platforms
- Provider plugins — new LLM providers (with full runtime hooks)
- Tool skills — skill directories shipped alongside tools
- Agent harnesses — alternative agent runtimes (e.g. Codex)

SDK overview: https://docs.openclaw.ai/plugins/sdk-overview
Building plugins: https://docs.openclaw.ai/plugins/building-plugins
Channel plugins: https://docs.openclaw.ai/plugins/sdk-channel-plugins
Provider plugins: https://docs.openclaw.ai/plugins/sdk-provider-plugins

## Testing

```bash
pnpm test                  # run all tests
pnpm test:e2e              # end-to-end tests
pnpm gateway:watch         # dev loop with auto-reload
```

CI guide: https://docs.openclaw.ai/ci

## Config Validation

```bash
openclaw config validate   # validate current config
openclaw config schema     # print full JSON Schema
pnpm config:docs:check     # detect drift between docs and schema (dev)
```

Config only accepts valid JSON5 — unknown keys or malformed values cause the Gateway to refuse to start. Only `openclaw doctor`, `openclaw logs`, `openclaw health`, `openclaw status` work until fixed.

## Useful Scripts

```bash
./scripts/clawlog.sh           # macOS unified logs
./scripts/docker/setup.sh      # Docker gateway setup
./scripts/sandbox-setup.sh     # Docker sandbox setup
```

## Install Methods

| Method | Command |
|--------|---------|
| Installer script | `curl -fsSL https://openclaw.ai/install.sh \| bash` |
| npm | `npm install -g openclaw@latest` |
| pnpm | `pnpm add -g openclaw@latest` |
| bun | `bun add -g openclaw@latest` |
| Nix | `nix profile install github:openclaw/nix-openclaw` |
| Docker | `./scripts/docker/setup.sh` |
| Homebrew | via tap |

Uninstall: `openclaw uninstall`
