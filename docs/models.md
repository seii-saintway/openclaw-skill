# Models & Providers

## Model Selection Order

1. Currently selected session model (e.g. via `/new <model>`)
2. `agents.defaults.model.fallbacks` (in order)
3. Configured primary model as final fallback

Model refs use `provider/model` format, e.g. `anthropic/claude-sonnet-4-6`.

## Config

```json5
{
  agents: {
    defaults: {
      model: {
        primary: "anthropic/claude-sonnet-4-6",
        fallbacks: ["openai/gpt-5.4"],
      },
      // If set, becomes the allowlist — unlisted models are rejected
      models: {
        "anthropic/claude-sonnet-4-6": { alias: "Sonnet" },
        "openai/gpt-5.4": { alias: "GPT" },
      },
      // Specialized models for specific tools
      imageModel: { primary: "anthropic/claude-sonnet-4-6" },
      pdfModel: { primary: "anthropic/claude-sonnet-4-6" },
      imageGenerationModel: { primary: "fal/flux-1-schnell" },
      musicGenerationModel: { primary: "..." },
      videoGenerationModel: { primary: "runway/gen4-turbo" },
    },
  },
}
```

"Model is not allowed" error means the selected model isn't in the `models` allowlist.

## CLI

```bash
openclaw models list                     # list available models
openclaw models status                   # model + auth profile status
openclaw models set anthropic/claude-sonnet-4-6
openclaw models scan                     # probe available models
openclaw onboard                         # interactive model + auth setup
```

## Supported Providers

| Provider | Model ref prefix | Auth |
|----------|-----------------|------|
| Anthropic | `anthropic/` | `ANTHROPIC_API_KEY` |
| OpenAI | `openai/` | `OPENAI_API_KEY` |
| Google | `google/` | `GOOGLE_API_KEY` |
| OpenCode (Codex) | `opencode/`, `codex/` | OAuth |
| DeepSeek | `deepseek/` | `DEEPSEEK_API_KEY` |
| Groq | `groq/` | `GROQ_API_KEY` |
| Mistral | `mistral/` | `MISTRAL_API_KEY` |
| xAI (Grok) | `xai/` | `XAI_API_KEY` |
| Ollama | `ollama/` | local |
| LM Studio | `lmstudio/` | local |
| OpenRouter | `openrouter/` | `OPENROUTER_API_KEY` |
| Together AI | `together/` | `TOGETHER_API_KEY` |
| Fireworks | `fireworks/` | `FIREWORKS_API_KEY` |
| AWS Bedrock | `bedrock/` | AWS credentials |
| Alibaba Qwen | `qwen/` | `DASHSCOPE_API_KEY` |
| Moonshot (Kimi) | `moonshot/` | `MOONSHOT_API_KEY` |
| MiniMax | `minimax/` | `MINIMAX_API_KEY` |
| Perplexity | `perplexity/` | `PERPLEXITY_API_KEY` |
| GitHub Copilot | `github-copilot/` | OAuth |
| Hugging Face | `huggingface/` | `HF_TOKEN` |
| NVIDIA | `nvidia/` | `NVIDIA_API_KEY` |
| LiteLLM | `litellm/` | proxy |
| vLLM | `vllm/` | local/remote |
| SGLang | `sglang/` | local/remote |
| Cloudflare AI Gateway | `cloudflare/` | proxy |
| Vercel AI Gateway | `vercel/` | proxy |

## Auth Profiles & Failover

OpenClaw uses **auth profiles** for both API keys and OAuth tokens. Profiles are per-agent: `~/.openclaw/agents/<id>/agent/auth-profiles.json`.

Failover flow:
1. Try current model with auth-profile rotation
2. If provider exhausted with failover-worthy error → next model in fallbacks
3. If all candidates fail → `FallbackSummaryError` with per-attempt detail

## Local Models

Ollama and LM Studio run locally:

```json5
{
  agents: {
    defaults: {
      model: { primary: "ollama/llama3.2" },
    },
  },
}
```

Start Ollama: `ollama serve` (default: `http://127.0.0.1:11434`)

Docs: https://docs.openclaw.ai/concepts/models
Providers: https://docs.openclaw.ai/concepts/model-providers
Failover: https://docs.openclaw.ai/concepts/model-failover
Local models: https://docs.openclaw.ai/gateway/local-models
