# What You Can Do With OpenClaw

A practical list of everything this setup lets you do.

---

## Coding

- Read any file in your workspace and explain it
- Write new functions, classes, or entire files from a description
- Refactor existing code — rename, restructure, simplify
- Add error handling, validation, or logging to existing code
- Review code and find bugs, security issues, or bad patterns
- Add tests for existing functions
- Fix failing tests automatically — run, read output, fix, repeat
- Convert code from one language to another
- Add type annotations or docstrings
- Implement a feature end-to-end across multiple files

---

## File and Folder Management

- Read, create, edit, and delete files in your workspace
- Search across files for a pattern or keyword
- Rename or reorganize files and folders
- Apply structured multi-file patches in one go
- Generate boilerplate for a new project or module
- Compare two files and explain the differences

---

## Terminal / Shell

- Run any shell command (with your approval every time)
- Run build tools — `npm run build`, `make`, `cargo build`, etc.
- Run tests and get a summary of what passed and failed
- Start and stop background processes
- Check running processes and logs
- Install packages or dependencies
- Run scripts and see their output
- Chain commands — build → test → deploy in one request

---

## Debugging

- Run a failing command, read the error, trace the root cause
- Add debug logging temporarily, run again, then clean it up
- Check environment variables and config files for issues
- Walk through a stack trace and pinpoint the exact problem
- Compare behavior before and after a change

---

## Web and Browser

- Open a URL in an isolated browser (not your personal browser)
- Read the content of any webpage
- Click, type, fill forms, and navigate pages automatically
- Take screenshots of pages
- Scrape structured data from websites
- Log into services and perform actions (with your approval)
- Download and read PDFs from the web

---

## Research and Summarization

- Summarize long documents or codebases
- Answer questions about your own code
- Explain unfamiliar libraries or APIs
- Look up documentation and bring back the relevant parts
- Compare two approaches or technologies
- Draft technical writeups or READMEs based on your code

---

## Memory and Context

- Remember preferences, facts, and decisions across sessions
- Recall things from past conversations via memory search
- Keep a daily log of what you worked on
- Build up a persistent knowledge base about your project over time

---

## Automation

- Set up recurring tasks (cron-style) that run on a schedule
- React to events — file changes, messages, webhooks
- Chain multi-step workflows into a single instruction
- Spawn sub-agents to run parallel tasks (when enabled)

---

## Messaging Channels (Optional)

If you connect a messaging app, you can do all of the above
by just sending a message from your phone or desktop:

- Telegram
- Discord
- Slack
- WhatsApp
- Signal
- iMessage / BlueBubbles
- Matrix, IRC, and many more

Example: send "run the tests and tell me if anything broke"
from your phone while you're away from your desk.

---

## Things to Be Careful With

These are possible but require extra attention:

- **Anything destructive** — always review before approving (delete, drop table, rm -rf)
- **Sensitive files** — switch to a local model before working with passwords, keys, or private data
- **External services** — the agent can call APIs, send messages, post to services if you give it access
- **Large refactors** — break into smaller steps, review each one before moving on

---

## Quick Reference: How to Ask for Things

```
# Coding
"Read src/auth.py and add rate limiting to the login endpoint"
"Find all TODO comments in the project and list them"
"Write unit tests for the UserService class"
"This function is slow, optimize it"

# Terminal
"Run npm test and fix whatever fails"
"Build the project and show me any errors"
"Check what's listening on port 3000"

# Files
"Create a new file called utils/validators.py with an email validator"
"Rename all snake_case files in /models to camelCase"

# Research
"Explain what this codebase does at a high level"
"What's the difference between JWT and session-based auth?"

# Memory
"Remember that we use PostgreSQL 15 and never use raw SQL"
"What did we decide about the auth approach last time?"
```
