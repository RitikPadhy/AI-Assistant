# Step 2 — Configure Models

## Get a Free Gemini API Key

1. Go to: https://aistudio.google.com/apikey
2. Sign in with your Google account
3. Click "Create API Key"
4. Copy the key

This is your primary model. Gemini 2.0 Flash is free, fast, and handles complex
agentic tasks (coding, file editing, tool use) well.

---

## Register Ollama with OpenClaw

Ollama doesn't need a real API key — any value works:

```bash
openclaw config set models.providers.ollama.apiKey "ollama-local"
```

---

## Register Gemini with OpenClaw

```bash
openclaw config set models.providers.google.apiKey "YOUR_GEMINI_KEY_HERE"
```

---

## Set Primary Model + Fallbacks

```bash
openclaw config set agents.defaults.model.primary "google/gemini-2.0-flash"
openclaw config set agents.defaults.model.fallbacks '["ollama/deepseek-r1:8b", "ollama/llama3.1:8b"]'
```

---

## How the Model Order Works

| Priority | Model | Where it runs | Why |
|---|---|---|---|
| 1st | `google/gemini-2.0-flash` | Google's servers (free) | Strongest, handles complex tasks best |
| 2nd | `ollama/deepseek-r1:8b` | Your RTX 3060 Ti | Kicks in if Gemini hits rate limits, has reasoning mode |
| 3rd | `ollama/llama3.1:8b` | Your RTX 3060 Ti | Last resort, always available offline |

OpenClaw automatically falls back when:
- Gemini hits its free tier rate limit
- Your internet is down
- Gemini returns an auth/billing error

It automatically returns to Gemini once the cooldown expires (1 min → 5 min → 25 min → 1 hour).

---

## Verify Models Are Detected

```bash
openclaw models list
```

You should see Gemini and both Ollama models listed.

---

## Force Local Only (When Needed)

If you're working with sensitive data and don't want anything leaving your machine,
switch to local mid-conversation:

```
/model ollama/deepseek-r1:8b
```

Switch back to normal:

```
/model google/gemini-2.0-flash
```
