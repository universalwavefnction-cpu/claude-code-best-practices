# 9. Git Workflow

Claude Code is fluent in git — it can stage, commit, branch, and open pull requests. Used well, this is a huge accelerator. A few habits keep it clean and safe.

## Let the agent handle the mechanics

You don't need to dictate git commands. Describe intent:

- "Commit this with a sensible message."
- "Create a branch for this feature and commit the changes."
- "Open a PR with a summary of what changed and why."

The agent writes clear commit messages, groups related changes, and can generate a PR body that actually explains the change. It reads the diff to do this, so the message reflects what really changed.

## Commit and push on *your* cue

A good default: **the agent commits when you ask, not automatically.** You want to review the diff before it's recorded. Two safe patterns:

- Do the work, review the diff, then say "commit this."
- Or let it commit locally as it goes, but **push only when you say so** — pushing is the point of no easy return.

If you're working on the default branch (`main`/`master`), have it branch first rather than committing straight to main.

## Review the diff — always

Before a commit lands, read what changed. The agent is good, but "good" isn't "infallible," and you're the one accountable for the code. `git diff` (or the diff shown in-session) is your checkpoint. This is also where you catch the accidental: a stray debug line, a file that shouldn't be tracked, a secret about to be committed.

## Keep secrets out of history

- Ensure `.gitignore` covers `.env`, credentials, and local config **before** committing.
- If a secret reaches a commit, removing the line in a later commit is not enough — it's still in history. **Rotate the secret.**
- Be especially careful with "commit everything" — check `git status` for files that shouldn't be tracked.

## Guard the dangerous operations

Some git actions are hard or impossible to undo. Keep a human in the loop for:

- **`git push --force`** and any history rewrite (`rebase` onto a shared branch, `reset --hard` on shared work).
- **Deleting branches** others may be using.
- **Force-pushing to a protected branch** — generally just don't.

Configure these in your permission deny-list (see [Permissions & Safety](05-permissions-safety.md)) so they can't happen without explicit approval.

## Good commit hygiene the agent supports

- **Small, focused commits** beat one giant blob. If you did three things, three commits.
- **Messages explain *why*,** not just *what* — the diff already shows what changed.
- **Don't mix** a refactor and a behavior change in one commit; they're hard to review and revert together.

Ask for this explicitly when it matters: "split this into logical commits" or "keep the refactor separate from the bug fix."

## Pull requests

When opening a PR, a useful body has: a one-line summary, the motivation, what changed at a high level, and how it was tested. Ask the agent to include these. It can also respond to review comments on a PR if you point it at them.

→ [10. Troubleshooting](10-troubleshooting.md)
