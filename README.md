# LiteLLM Gateway

Central AI gateway for all dxtechph.online apps. Routes LLM requests through a single OpenAI-compatible endpoint with cost tracking, caching, and virtual key management.

**Endpoint:** `http://litellm:4000/v1/chat/completions` (internal) or `https://llm-gateway.dxtechph.online/v1/chat/completions` (public)

## Architecture

```
App → LiteLLM Proxy → OpenRouter (OpenCode sub) → models
                    → Future: OpenAI, Anthropic, etc.
```

## Providers Configured

| Model | Source | Rate Limit |
|---|---|---|
| `deepseek-v4-flash` | OpenRouter | 60 RPM |
| `deepseek-v4-pro` | OpenRouter | 30 RPM |

## Adding a New Provider

Add a block to `config.yaml` model_list, add the API key to Dokploy env vars, then redeploy.

## Virtual Keys

Create per-app keys via the LiteLLM Admin UI at `/admin` after deployment:
- Law Bar → restricted to flash/pro only, $5/month budget
- NEXIAM → unrestricted
- Notes → flash only, $1/month budget

## First-Time Setup

1. Copy `.env.example` to `.env` and fill in values
2. Generate master key: `openssl rand -base64 32`
3. Deploy via Dokploy
4. Create virtual keys via Admin UI
5. Point apps to `http://litellm:4000/v1/chat/completions`
