# Step 4 — Set Up Your Workspace

The workspace is the folder OpenClaw treats as its home base.
It reads files from here, writes files here, and automatically loads
context files from here on every conversation.

---

## 1. Point OpenClaw at Your Project

```bash
openclaw config set agents.defaults.workspace "/path/to/your/project"
```

Keep this focused — point it at a specific project, not your entire home folder.
The more focused the workspace, the less noise in the agent's context.

---

## 2. Create Bootstrap Files

These files are automatically injected into every conversation as context.
The agent reads them on every message, so it always knows your setup
without you having to repeat yourself.

Create these files in the root of your workspace:

---

### `AGENTS.md` — Rules and Behavior

This is the most important file. It tells the agent how to behave.

```markdown
# Agent Rules

## General
- Always ask before deleting any file
- Always explain what you're about to do before doing it
- Stay within the workspace directory at all times
- If unsure about something, ask — don't guess

## Coding
- Use [your language/framework here]
- Follow [your style guide or conventions]
- Write tests for new functions
- Never commit .env files or secrets

## Safety
- If a task feels risky or destructive, stop and confirm first
- Never run commands that affect files outside the workspace
```

---

### `USER.md` — About You and Your Stack

```markdown
# About Me

## Setup
- OS: Windows with WSL2
- Hardware: RTX 3060 Ti (8GB VRAM), 24GB RAM
- Running models locally via Ollama as fallback

## Tech Stack
- [Fill in your languages, frameworks, databases]

## Preferences
- Concise code over verbose code
- No unnecessary comments
- Explain changes briefly after making them
```

---

### `MEMORY.md` — Long-term Facts

Things the agent should always remember across sessions.
Start with just a few key facts and add to it over time:

```markdown
# Memory

- Workspace is at /path/to/your/project
- Local dev server runs on port [your port]
- Never commit .env files
- [Any other permanent facts about your setup]
```

---

## 3. Check What's Being Injected

After starting the gateway, send this command to the agent:

```
/context list
```

This shows exactly what files are loaded into context and how many tokens
each one is using. If a file is too large it will be truncated — keep
bootstrap files short and focused.

---

## 4. Tell the Agent to Remember Things

During conversations, tell the agent to save important things:

> "Remember that we use port 5432 for the database"

> "Remember to always run migrations before tests"

It will write these to `MEMORY.md` and they'll persist across sessions.

---

## 5. Keep Context Lean

Your local models (7-8B) have small context windows (~8k tokens).
If the context fills up, the agent starts forgetting earlier things.

- `/compact` — summarizes old history to free up space
- `/status` — shows how full the context window is
- Work on one file or task at a time
- Keep `AGENTS.md`, `USER.md`, and `MEMORY.md` short and precise
