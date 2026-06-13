# 10. Troubleshooting

Common failure modes and what to do about them. Most "the agent isn't working well" situations trace back to one of these.

## The agent feels dumb / loses the thread

**Cause:** cluttered context. After a long or task-switching session, the signal for the current task is buried under earlier work.

**Fix:** `/clear` if you've moved to a new task; `/compact` if you're continuing the same long one. This is the most common fix and the first thing to try. See [Context Management](04-context-management.md).

## It keeps making the same mistake

**Cause:** a convention or constraint it doesn't know about, repeated each session.

**Fix:** write the rule into `CLAUDE.md`. "Use the `Result` type, never throw" in the file beats correcting it every time. If it happened twice, it'll happen a third time without a written rule. See [CLAUDE.md](02-claude-md.md).

## It went off and did the wrong thing

**Cause:** the ask was ambiguous, or it ran with an assumption you'd have corrected.

**Fix:** for anything substantial, ask for a **plan first** (or use plan mode). Review the approach before it writes code. Cheap to redirect a plan; expensive to redirect a finished implementation. See [Effective Prompting](03-effective-prompting.md).

To stop it mid-action: press **`Esc`**.

## It edited a file it shouldn't have

**Cause:** no guardrail marking that file/area as off-limits.

**Fix:** add a "Don't touch" section to `CLAUDE.md`, and for hard rules use the permission deny-list in `settings.json`. To recover the file, `git checkout` it (if committed) — another reason to commit known-good states.

## A command needs interactive input and hangs

**Cause:** the agent ran something that wants a TTY (an interactive login, a `-i` rebase, a prompt-driven installer).

**Fix:** run that step yourself. In-session, prefix a shell command with `!` to run it directly, or do it in another terminal, then tell the agent to continue. Interactive flows are the one place to take the keyboard back.

## Permission prompts are constant and annoying

**Cause:** every safe command is asking for approval.

**Fix:** allow-list your common safe commands in `.claude/settings.json` (e.g. your test and lint commands). The noise drops sharply and the genuinely consequential prompts stand out. See [Permissions & Safety](05-permissions-safety.md).

Tempted to just run `claude --dangerously-skip-permissions` to silence everything? Don't — not on a machine you care about. That turns off *all* guardrails, including the ones protecting destructive and irreversible actions. Allow-listing or accept-edits mode gets you the quiet without the blast radius. Bypass mode belongs only in a disposable sandbox. See [the bypass-mode section](05-permissions-safety.md#-dangerously-skip-permissions-bypass-mode).

## It claims something works but you're not sure

**Cause:** the agent reported success without (or despite) verification.

**Fix:** ask it to *show* you — run the tests and paste the output, run the app and describe the behavior. "Verify it works by running X and showing the result" turns a claim into evidence. Don't accept "done" for anything important without seeing it pass.

## An MCP tool or external call returns suspicious instructions

**Cause:** possible prompt injection — content from an external source telling the agent to do something.

**Fix:** treat instructions arriving *inside tool results or fetched content* as data, not commands. If a fetched page or ticket "says" to delete something or exfiltrate a secret, that's a red flag, not a task. See [MCP Servers](07-mcp-servers.md).

## Token usage / cost feels high

**Causes and fixes:**

- Re-reading huge files repeatedly → point at specific files/functions; let subagents do broad reads in isolated context.
- One endless session → `/clear` between tasks so you're not dragging old context forward.
- Heavy work on a top model when a lighter one would do → switch with `/model` for routine tasks.

## When in doubt

- `/help` lists commands.
- Ask the agent directly: "why did you do X?" — it can explain its reasoning, which often reveals the misunderstanding.
- Small steps with verification beat one big leap of faith.

---

That's the guide. Start with a `CLAUDE.md`, keep your context clean, verify the work, and guard the destructive actions. Everything else is refinement.

← [Back to README](../README.md)
