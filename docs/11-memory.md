# 11. Memory — Making Claude Remember

Each session starts fresh: the agent doesn't recall your last conversation. **Memory** is how you carry knowledge forward — conventions, decisions, preferences — so you're not re-explaining the same things every time. [`CLAUDE.md`](02-claude-md.md) is the core of this; here's the full system and how to use it well.

## The memory hierarchy

Claude Code reads memory from several locations and layers them, most-specific on top:

| Level | Location | Scope | Commit it? |
|-------|----------|-------|-----------|
| **Project** | `./CLAUDE.md` (repo root) | This project, shared with the team | Yes |
| **Subdirectory** | `./some/dir/CLAUDE.md` | Applies when working in that folder | Yes |
| **User (global)** | `~/.claude/CLAUDE.md` | You, across *all* projects | No — it's yours |
| **Local / private** | `./CLAUDE.local.md` | This project, just you | No — gitignore it |

Use each for what it's for:

- **Project** — build/test commands, architecture rules, conventions everyone shares.
- **User (global)** — your personal working preferences: "always explain the plan before a big refactor," "prefer functional style." These follow you into every repo.
- **Local** — project notes you don't want to push (a scratch endpoint, a personal reminder).

More specific layers override more general ones, so a project rule beats a global preference when they conflict.

## Quick-capture with `#`

The fastest way to record something mid-session: type `#` followed by the note.

```
#always run the linter before committing
```

Claude offers to write it into the appropriate memory file. This is the habit that makes memory *grow*: the moment you notice "I keep having to tell it this," capture it with `#` and you never tell it again.

## Editing memory with `/memory`

Run `/memory` to open your memory files in your editor for a proper cleanup — restructuring, deleting stale lines, fixing a rule that changed. Quick-capture is for adding one thing fast; `/memory` is for tidying.

## Imports with `@`

A `CLAUDE.md` can pull in other files with `@path`:

```markdown
See @docs/architecture.md for the module layout.
Follow the conventions in @./STYLEGUIDE.md.
```

This keeps `CLAUDE.md` lean while still giving the agent the detail when relevant — you reference the deep docs instead of copying them in. Useful in monorepos and large projects where one flat file would balloon.

## Hygiene — the practices that keep memory useful

Memory rots if you only ever add to it. The discipline:

1. **Keep it lean.** Every line of memory is read on every turn, spending context. A bloated `CLAUDE.md` buries the rules that matter and slows the agent. Aim for scannable. If a section is no longer load-bearing, cut it.
2. **Be specific, not generic.** "Write good code" is noise. "Return a `Result` type from fallible functions, never throw" is a rule the agent can actually follow. Memory earns its place by being concrete.
3. **Update, don't pile on.** When a convention changes, *edit the existing line* — don't add a contradicting one below it. Two rules that disagree make the agent guess. One current rule beats a history of them.
4. **Prune what's wrong.** A stale memory is worse than none — it actively misleads. When something turns out incorrect or obsolete, delete or fix it the moment you notice.
5. **Use absolute dates.** "Fix this after the next release" is meaningless in three months. Write "after the 2.0 release (target March 2026)." Memory outlives the moment it was written.
6. **Never store secrets.** No API keys, tokens, passwords, or connection strings in any committed memory file. Memory is read every session and (for project/subdirectory files) committed to the repo. Secrets live in `.env` and your secret manager.

## Memory beyond `CLAUDE.md`

For knowledge that should persist across many sessions and grow over time — a running log of decisions, an index of how a system works — some workflows keep a **dedicated memory directory** of small Markdown notes, one topic per file, with a single index file that lists them. The same hygiene applies, and it scales: each fact has a home, the index makes it findable, and you update the relevant file rather than duplicating. Whether you use a single `CLAUDE.md` or a directory of notes, the principles are identical — **specific, current, lean, no secrets.**

## The one-sentence version

Memory is a teammate's onboarding doc that you improve every time it trips on something it should have known. Capture with `#`, tidy with `/memory`, keep it lean and current, and never put a secret in it.

← [Back to README](../README.md) · See also [2. CLAUDE.md](02-claude-md.md)
