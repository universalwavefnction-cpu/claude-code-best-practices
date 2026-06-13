# 1. Getting Started

## Install

Claude Code runs on Node.js 18+. Install it globally:

```bash
npm install -g @anthropic-ai/claude-code
```

Then start it inside any project directory:

```bash
cd your-project
claude
```

On first run it walks you through authentication (a Claude subscription or API key). After that, `claude` drops you into an interactive session in the current folder.

You can also run it non-interactively for scripting:

```bash
claude -p "summarize what this repo does"   # print mode, one-shot
```

## The core loop

Working with Claude Code is a conversation with a teammate who can act. The loop is:

1. **You describe a goal** — a bug to fix, a feature to add, a question about the code.
2. **The agent explores** — reads files, searches, runs commands to understand the situation.
3. **The agent acts** — edits files, runs tests, makes commits.
4. **You review** — read the diff, check the output, redirect if needed.

The agent keeps you in the loop on consequential actions and asks before doing anything destructive. Your job shifts from *typing every line* to *steering and verifying*.

## Your first session

Start small to build trust in the tool. Good first asks:

- "Explain what this project does and how it's structured."
- "Find where user authentication is handled."
- "Add a test for the `parseDate` function."

Watch how it works: it reads before it writes, explains its plan, and shows you diffs. Once you've seen it handle a few small tasks correctly, you'll know how much rope to give it.

## Essential in-session commands

| Command | What it does |
|---------|-------------|
| `/init` | Scan the repo and generate a starter `CLAUDE.md` |
| `/clear` | Wipe the conversation context — use between unrelated tasks |
| `/help` | List available commands |
| `/model` | Switch the model (e.g. to a faster or more capable one) |
| `/config` | Open settings |
| `/memory` | Open your memory files to edit and tidy them |
| `Esc` | Interrupt the agent mid-action |
| `!command` | Run a shell command directly (output goes into the session) |
| `#text` | Quickly add a note to memory / `CLAUDE.md` |

## What to do next

The single best first move in a new project is to run `/init` and then refine the generated `CLAUDE.md` by hand. That file is covered next — it's the highest-leverage thing in this whole guide.

→ [2. CLAUDE.md — Project Memory](02-claude-md.md)
