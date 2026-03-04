# Step 6 — How It All Works Together

This file explains the full flow so you understand what's happening
at each stage when you use OpenClaw.

---

## The Big Picture

```
You (message)
    ↓
Gateway (running locally on your machine)
    ↓
Reads workspace bootstrap files (AGENTS.md, USER.md, MEMORY.md)
    ↓
Picks a model (Gemini → deepseek fallback → llama fallback)
    ↓
Model thinks + decides what tools to use
    ↓
Tools run on your machine (read files, run commands, browse web)
    ↓
Results fed back to model
    ↓
Model replies to you
```

---

## Stage 1: You Send a Message

You talk to the agent through whatever interface you're using —
the CLI, a connected messaging app (Telegram, Discord, etc.), or the web UI.

Your message goes to the **Gateway**, which is a process running locally
on your machine. Nothing goes to the internet yet at this stage.

---

## Stage 2: Gateway Builds Context

Before sending anything to a model, the Gateway assembles the full context:

- Your bootstrap files (`AGENTS.md`, `USER.md`, `MEMORY.md`) — injected automatically
- The conversation history for this session
- Tool definitions (what tools the agent is allowed to use)
- Current time, workspace location, runtime info

This is what the model will "see" when it processes your message.

---

## Stage 3: Model Is Selected

The Gateway picks the best available model in order:

```
Is Gemini 2.0 Flash available and not rate-limited?
    YES → send to Google's servers
    NO  → is deepseek-r1:8b loaded in Ollama?
              YES → send to your RTX 3060 Ti (fully local)
              NO  → fall back to llama3.1:8b on your GPU
```

For Gemini: your context + message travel to Google's API over HTTPS.
For Ollama models: everything stays entirely on your machine.

---

## Stage 4: Model Thinks and Uses Tools

The model receives your message and context, then decides what to do.
For simple questions it just replies. For tasks it uses tools:

**Reading a file:**
```
You: "Read auth.py and find any security issues"
Model: calls read("src/auth.py")
Gateway: reads the file from your disk
Result: file content sent back to model
Model: analyzes and replies
```

**Running a command:**
```
You: "Run the tests and fix any failures"
Model: calls exec("pytest tests/")
Gateway: prompts YOU for approval (because ask=always)
You: approve
Gateway: runs the command, captures output
Result: test output sent back to model
Model: reads failures, edits files to fix them, runs again
```

**Editing a file:**
```
You: "Add input validation to the login endpoint"
Model: calls read("routes/auth.py") → reads current code
Model: calls edit("routes/auth.py", ...) → makes the change
Gateway: writes to disk
Model: confirms what it changed
```

The model can chain these — read, edit, run, read output, fix, run again —
all in one go without you babysitting each step.

---

## Stage 5: Approval Gates

Because you set `tools.exec.ask = "always"`, every shell command
triggers an approval prompt before it runs:

```
Agent wants to run: pytest tests/
Working directory: /your/project
[Allow once] [Always allow] [Deny]
```

You stay in control. The agent cannot run anything on your machine
without you seeing and approving it first.

For file reads and edits inside your workspace, no approval is needed —
that's the tradeoff you made by setting a specific workspace.

---

## Stage 6: Memory and Persistence

At the end of a long session (or when context fills up), OpenClaw
automatically prompts the model to save important things to `MEMORY.md`
before compacting the conversation.

Next time you start a session, the agent reads `MEMORY.md` and picks up
where it left off — it knows your preferences, your project structure,
and any facts you told it to remember.

Daily notes are written to `memory/YYYY-MM-DD.md` automatically.

---

## What Runs Where — Summary

| Component | Runs on |
|---|---|
| Gateway | Your machine (always) |
| Ollama models | Your RTX 3060 Ti (always local) |
| Gemini 2.0 Flash | Google's servers (your messages go there) |
| File reads/writes | Your machine |
| Shell commands | Your machine (after your approval) |
| Memory files | Your machine (`~/.openclaw/workspace/`) |

---

## When to Use Local vs Cloud Models

| Situation | Use |
|---|---|
| Normal coding, general tasks | Gemini (default, automatic) |
| Sensitive code, passwords, private data | `/model ollama/deepseek-r1:8b` |
| No internet / offline work | Ollama kicks in automatically |
| Complex reasoning / debugging | deepseek-r1:8b (has thinking mode) |
| Rate limited by Gemini | Ollama kicks in automatically |

---

## The Full Cost Breakdown

| Resource | Cost |
|---|---|
| Gemini 2.0 Flash (free tier) | $0 |
| Ollama + local models | $0 |
| OpenClaw itself | $0 (open source) |
| Electricity for GPU | Minimal (only during inference) |

**Total: $0/month** for personal daily use within Gemini's free tier limits.
