# 2. CLAUDE.md — Project Memory

If you do one thing from this guide, do this: write a `CLAUDE.md`.

## What it is

`CLAUDE.md` is a plain-Markdown file in your project root that Claude Code reads automatically at the start of every session. It's the project's persistent memory — the place to put everything you'd otherwise have to re-explain each time.

Think of onboarding a new engineer. What would you tell them on day one? The build command, the test command, the conventions you care about, the parts of the codebase that are load-bearing and fragile. That's exactly what goes in `CLAUDE.md`.

## How to create one

Run `/init` inside your project. Claude scans the codebase and drafts a `CLAUDE.md` for you. Then **edit it by hand** — the generated draft is a starting point, not the finished article. The value comes from the specific, hard-won rules only you know.

## What to put in it

Good `CLAUDE.md` content is **specific, actionable, and non-obvious**:

```markdown
# Project: Acme API

## Commands
- Build: `npm run build`
- Test: `npm test` (single file: `npm test -- path/to/file.test.ts`)
- Lint: `npm run lint` — must pass before any commit
- Dev server: `npm run dev` (port 3000)

## Conventions
- TypeScript strict mode. No `any` without a `// reason:` comment.
- Use the existing `Result<T, E>` type for fallible functions — don't throw.
- Tests live next to source as `*.test.ts`.

## Architecture notes
- `src/core/` is the domain layer — pure, no I/O. Keep it that way.
- All DB access goes through `src/db/repository.ts`. Don't query directly.

## Don't touch
- `src/legacy/` — being deprecated, do not extend.
- `migrations/` — append-only. Never edit an existing migration.
```

## What NOT to put in it

- **Secrets.** No API keys, tokens, passwords, or connection strings. `CLAUDE.md` is committed to the repo and read every session.
- **The obvious.** Don't restate what the code already makes clear. "This is a React project" adds nothing; "we use Server Components, never `useEffect` for data fetching" is gold.
- **Novels.** Keep it tight. A bloated `CLAUDE.md` costs context on every turn and buries the rules that matter. Aim for scannable.

## Scope and layering

You can have `CLAUDE.md` files at multiple levels:

- **Project root** — committed, shared with the team. The main one.
- **Subdirectories** — a `CLAUDE.md` in a subfolder applies when working there. Useful for monorepos where each package has its own conventions.
- **Global** (`~/.claude/CLAUDE.md`) — your personal preferences across all projects (e.g. "explain your plan before large refactors").

More specific files layer on top of more general ones.

## Keep it alive

Treat `CLAUDE.md` as a living document. When the agent makes the same mistake twice, that's a signal: add a rule so it doesn't make it a third time. When a convention changes, update the file. A stale `CLAUDE.md` is worse than none — it actively misleads.

A fast way to add a rule mid-session: type `#` followed by the note, and Claude will offer to write it into memory.

→ [3. Effective Prompting](03-effective-prompting.md)
