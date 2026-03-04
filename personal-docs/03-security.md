# Step 3 — Security Hardening

Apply every setting in this file before using OpenClaw for real tasks.
These are not optional — the defaults are intentionally permissive and
need to be tightened for safe daily use.

---

## 1. Enable Sandboxing

Sandboxing is OFF by default. This means the agent runs commands directly
on your machine with no isolation. Enable it:

```bash
openclaw config set agents.defaults.sandbox.mode "non-main"
```

`non-main` means: sandbox all agents except the main one. This is the
recommended middle ground — you keep capability while isolating sub-agents.

For maximum isolation (may break some features):

```bash
openclaw config set agents.defaults.sandbox.mode "all"
```

---

## 2. Lock File Access to Your Workspace

Prevent the agent from reading or writing outside your project folder:

```bash
openclaw config set tools.fs.workspaceOnly true
openclaw config set tools.exec.applyPatch.workspaceOnly true
```

Without this, the agent could theoretically read any file on your machine.

---

## 3. Require Approval Before Running Shell Commands

Never let the agent run shell commands silently:

```bash
openclaw config set tools.exec.security "allowlist"
openclaw config set tools.exec.ask "always"
```

- `security=allowlist` — only explicitly approved binaries can run
- `ask=always` — prompts you before every command, no exceptions

You'll get a confirmation dialog every time the agent tries to run something.
You can approve once, approve always, or deny.

---

## 4. Keep the Gateway Loopback-Only

The control UI and API should never be exposed to the internet:

```bash
openclaw config set gateway.bind "loopback"
```

This locks the gateway to `127.0.0.1` — only your own machine can reach it.

If you need remote access, use an SSH tunnel or Tailscale.
Never bind to `0.0.0.0` or expose it via a public reverse proxy.

---

## 5. Disable Sub-agent Spawning (Unless You Need It)

Sub-agents can run things in parallel without you seeing each step:

```bash
openclaw config set tools.deny '["sessions_spawn"]'
```

Re-enable only if you explicitly need multi-agent workflows.

---

## 6. Lock Down Who Can Message the Agent

If you connect any messaging channels (Telegram, Discord, etc.), whitelist
only your own account:

```json5
// In ~/.openclaw/openclaw.json
{
  channels: {
    telegram: { allowFrom: ["+YOUR_PHONE_NUMBER"] },
    discord:  { allowFrom: ["YOUR_DISCORD_USER_ID"] }
  }
}
```

Without this, anyone who finds your bot can talk to it.

---

## 7. Run the Built-in Security Audit

```bash
openclaw security audit --deep
```

This scans your config for risky settings and tells you exactly what to fix.

Auto-fix safe issues:

```bash
openclaw doctor --fix
```

---

## 8. Important Rules to Follow

- **One person, one gateway.** Never share your gateway with others.
  If someone else needs OpenClaw, they get their own machine/VPS.

- **Sensitive data = local models only.** Passwords, private keys, personal files —
  switch to `ollama/deepseek-r1:8b` before working with anything sensitive.
  Nothing sent to Gemini leaves your machine.

- **Only install plugins you trust.** Plugins run with the same permissions
  as the gateway itself — treat them like installing software.

- **Keep Node.js updated.** OpenClaw requires Node v22.12.0+ for security patches.
