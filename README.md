# pi-nanogpt-provider

A Pi extension that registers [NanoGPT](https://nano-gpt.com) as a model provider, giving you access to 600+ LLMs through a single API key.

## Features

- **Dynamic model discovery** — Fetches the full model catalog from NanoGPT's `/v1/models` endpoint at startup
- **Heuristic specs** — Context windows, max tokens, and vision/reasoning support inferred from model naming patterns for all models
- **Reasoning support** — Models with `:thinking` suffix or known reasoning families are flagged automatically
- **Vision support** — Multimodal models (GPT-4o, Claude, Gemini, Qwen-VL, etc.) are detected from naming patterns

## Install

```bash
pi install git:github.com/markg85/pi-nanogpt-provider
```

## Configuration

### Option 1: `/login` (recommended)

Run Pi's built-in login command to store your API key in Pi's credential store:

```
/login nanogpt
```

You'll be prompted for your NanoGPT API key. The key is validated against the API and stored securely in `~/.pi/agent/auth.json`. No environment variable needed.

### Option 2: Environment variable

```bash
export NANOGPT_API_KEY="your-api-key"
```

Or add it to your Pi settings (`.pi/settings.json` or `~/.pi/agent/settings.json`):

```json
{
  "env": {
    "NANOGPT_API_KEY": "your-api-key"
  }
}
```

> **Note:** Credentials from `/login` take priority over the environment variable.

## Usage

After installation, NanoGPT models are available in Pi:

```bash
# List all NanoGPT models
pi --list-models | grep nanogpt

# Use a specific model
pi --model nanogpt:openai/gpt-5.2

# Use a thinking model
pi --model nanogpt:deepseek/deepseek-v4-pro:thinking
```

## Model Selection

NanoGPT supports model ID suffixes for routing preferences:

| Suffix | Effect |
|--------|--------|
| `:fast` / `:speed` | Fastest completion |
| `:cheap` / `:price` / `:floor` | Cheapest provider |
| `:throughput` | Highest tokens per second |
| `:latency` | Lowest time to first token |
| `:tools` | Tools-capable routing |
| `:thinking` | Enable reasoning/thinking mode |
| `:thinking:low` | Low thinking budget |
| `:thinking:medium` | Medium thinking budget |
| `:thinking:high` / `:thinking:max` | Maximum thinking budget |

Example: `pi --model nanogpt:openai/gpt-5.2:fast`

## Supported API

NanoGPT uses the **OpenAI Chat Completions** compatible API at `https://nano-gpt.com/api/v1`, so this extension uses Pi's `openai-completions` streaming implementation.

## License

MIT
