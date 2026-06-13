# 4. Context Management

The agent reasons over a context window — the running record of your conversation, the files it has read, and the command output it has seen. That window is finite. Managing it well is the difference between a sharp agent and a confused one.

## Why it matters

Two failure modes come from poor context hygiene:

1. **Dilution.** When the context is full of unrelated earlier work, the signal for the current task gets buried. The agent starts referencing things that no longer matter.
2. **Drift.** On a long, meandering session the agent can lose track of the original goal or carry stale assumptions from a task you finished an hour ago.

A focused context is a sharp agent. Most "the agent got dumb" moments are actually "the context got cluttered" moments.

## Clear between unrelated tasks

The single most useful habit: run `/clear` when you switch to something unrelated.

Finished fixing the auth bug and now you want to update the build config? `/clear` first. It wipes the conversation so the new task starts clean. Your `CLAUDE.md` and project files are untouched — only the conversation resets.

Rule of thumb: **one logical task per context.** If the next thing shares nothing with the last thing, clear.

## Long single tasks: let compaction work

For a genuinely long *single* task, you don't need to babysit context. When it fills up, Claude Code automatically **compacts** — it summarizes the conversation so far and continues with the summary plus recent detail. The thread of work survives; you don't have to wrap up early.

You can also trigger this yourself with `/compact` if you want to consolidate before continuing.

## Be deliberate about what you load

Reading a 5,000-line file to answer a one-line question spends context you'll want later. You don't usually need to manage this manually — the agent and its search tools are good at reading just the relevant parts — but it's worth knowing: pointing the agent at *specific* files or functions is cheaper and sharper than "go read the whole codebase."

## Delegate heavy reading to subagents

When a task requires sweeping across many files (e.g. "find every place we call the payments API"), that's a lot of file content to pull into your main context. Subagents solve this: a subagent does the searching in its *own* context and returns just the conclusion to you. Your main context stays clean. See [Subagents & Workflows](08-subagents-workflows.md).

## Signs your context needs a reset

- The agent references a file or decision from a task you already finished.
- It re-asks something you answered earlier.
- Responses feel less precise than at the start of the session.

When you see these, `/clear` (if switching tasks) or `/compact` (if continuing the same one).

→ [5. Permissions & Safety](05-permissions-safety.md)
