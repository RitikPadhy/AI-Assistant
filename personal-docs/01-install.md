# Step 1 — Install Everything

## Prerequisites

- Windows with WSL2 (strongly recommended for OpenClaw on Windows)
- Node.js v22.12.0 or later
- npm, pnpm, or bun

Verify Node version:

```bash
node --version
# Should output v22.12.0 or later
```

---

## Install Ollama

Download and install from: https://ollama.ai

Verify it's running:

```bash
ollama --version
```

Pull the local models you'll use as fallbacks:

```bash
ollama pull deepseek-r1:8b
ollama pull llama3.1:8b
```

Verify models are available:

```bash
ollama list
```

---

## Install OpenClaw

```bash
npm install -g openclaw@latest
```

Run the onboarding wizard — this sets up the gateway as a background daemon (auto-starts with your system):

```bash
openclaw onboard --install-daemon
```

Verify the install:

```bash
openclaw --version
```
