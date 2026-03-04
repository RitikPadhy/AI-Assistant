# Step 5 — Start and Verify

## Start the Gateway

```bash
openclaw gateway --port 18789 --verbose
```

`--verbose` shows logs so you can see what's happening on first run.
Once you're comfortable, you can drop `--verbose`.

If you installed the daemon in Step 1, the gateway starts automatically
with your system and you don't need to run this manually every time.

Check if the daemon is running:

```bash
openclaw gateway status
```

---

## Verify Models Are Working

```bash
openclaw models list
```

Expected output — you should see something like:

```
google/gemini-2.0-flash     ✓ available
ollama/deepseek-r1:8b       ✓ available
ollama/llama3.1:8b          ✓ available
```

If a model shows as unavailable, check:
- Ollama is running: `ollama serve`
- Gemini key is set: `openclaw config get models.providers.google.apiKey`

---

## Run a Health Check

```bash
openclaw doctor
```

This checks your full setup — models, config, security, daemon, workspace.
Fix anything it flags before using OpenClaw for real work.

---

## Run the Security Audit

```bash
openclaw security audit --deep
```

Make sure there are no high-severity findings before continuing.

---

## Send a Test Message

```bash
openclaw agent --message "Hello, what model are you running on?"
```

The agent should respond and tell you it's using Gemini 2.0 Flash.

Test a fallback manually:

```bash
openclaw agent --message "Hello" --model "ollama/deepseek-r1:8b"
```

---

## Test File Access

In your workspace folder, ask the agent to read a file:

```
Read the file README.md and summarize it
```

If it works, file access is configured correctly.

---

## Useful Commands to Know

```bash
/status          # show current model, context window usage, session info
/model           # list available models
/context list    # show what's loaded into context and token counts
/compact         # compress old conversation history to free context space
/new             # start a fresh session
```
