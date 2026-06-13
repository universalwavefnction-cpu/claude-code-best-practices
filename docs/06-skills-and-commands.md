# 6. Skills & Slash Commands

Two mechanisms let you package repeatable work so you (and the agent) don't reinvent it each time: **slash commands** and **skills**.

## Slash commands

A slash command is a saved prompt you invoke with `/name`. Built-ins include `/clear`, `/init`, `/help`, `/model`, `/compact`. You can add your own.

### Creating a custom command

Drop a Markdown file in `.claude/commands/` (project-level, shared with the team) or `~/.claude/commands/` (personal, all projects). The filename is the command name.

`.claude/commands/review.md`:

```markdown
---
description: Review the current diff for bugs and style issues
---

Review the staged and unstaged changes in this repo. For each file:
- Flag correctness bugs and edge cases.
- Note anything that violates the conventions in CLAUDE.md.
- Suggest simplifications, but only high-confidence ones.

Be concise. Group findings by severity.
```

Now `/review` runs that prompt anytime. Commands can take arguments — reference them with `$ARGUMENTS` (or `$1`, `$2`) and the agent substitutes what you type after the command.

`.claude/commands/test-file.md`:

```markdown
---
description: Write tests for a given file
---

Write thorough unit tests for `$ARGUMENTS`, following the existing
test patterns in this project. Cover edge cases and error paths.
```

Invoke with `/test-file src/utils/parse.ts`.

### When to make one

Anytime you find yourself typing the same multi-line instruction repeatedly, make it a command. Common ones teams build: `/review`, a release-notes generator, a "explain this module" helper, a project-specific scaffolder.

## Skills

Skills are a step up: packaged capabilities — instructions plus optional scripts and resources — that the agent loads **automatically when relevant**, based on a description you write. Where a slash command is something *you* invoke, a skill can trigger itself when the task matches.

A skill lives in its own folder with a `SKILL.md` that has frontmatter describing when to use it:

```markdown
---
name: changelog-writer
description: Generate a changelog entry from recent commits. Use when the
  user asks to update the changelog or prepare release notes.
---

# Changelog Writer

Steps:
1. Run `git log` since the last release tag.
2. Group commits by type (feat, fix, chore).
3. Write entries in the style of the existing CHANGELOG.md.
...
```

When a request matches that description, the agent pulls the skill in and follows it — no manual invocation needed. Skills can bundle helper scripts the agent runs, reference templates, and encode multi-step procedures that are too involved to retype.

### Skill vs command — which to use

- **Slash command** — a prompt you trigger deliberately. Simple, fast to make.
- **Skill** — a richer capability that auto-triggers and can carry scripts/files. Use when the procedure is involved or you want it to fire automatically on matching tasks.

## Don't reinvent what exists

Before building a helper, check whether a skill or command already covers it. A pile of overlapping near-duplicate commands is its own kind of mess. Keep the set small, named clearly, and documented in your `CLAUDE.md` so the team knows what's available.

→ [7. MCP Servers](07-mcp-servers.md)
