# 5. Permissions & Safety

Claude Code can run commands and edit files. That power is the point — and it's why you control what it's allowed to do without asking.

## How permissions work

By default, the agent asks before actions that change things — running a command, editing a file, making a commit. You approve or deny each one. As you build trust (and for repetitive safe actions), you can pre-approve categories so it stops asking for those.

When prompted, you typically get options like:

- **Yes** — allow this once.
- **Yes, and don't ask again** — allow this kind of action for the session (or persist it).
- **No** — deny, and tell the agent what to do instead.

A denied action is feedback. The agent adjusts rather than retrying the same thing.

## Permission modes

Claude Code offers modes that set the default posture:

| Mode | Behavior | When to use |
|------|----------|-------------|
| **Default** | Asks before edits and commands | Normal work |
| **Plan mode** | Read-only; researches and proposes, never edits until you approve | Scoping a change, exploring unfamiliar code |
| **Accept edits** | Auto-approves file edits, still gates commands | Trusted, well-scoped editing tasks |
| **Bypass / "yolo"** | Asks for nothing | Only in throwaway sandboxes — never on anything you care about |

Switch modes with `/config` or the mode shortcut. **Plan mode** is underused and excellent: it lets the agent do all its investigation and hand you a plan with zero risk of it changing something first.

## Configure allow/deny lists

For repeatable safety, configure permissions in `settings.json` (project-level at `.claude/settings.json`, or personal). You can pre-allow safe commands and pre-deny dangerous ones:

```json
{
  "permissions": {
    "allow": [
      "Bash(npm run test:*)",
      "Bash(git status)",
      "Bash(git diff:*)"
    ],
    "deny": [
      "Bash(rm -rf:*)",
      "Bash(git push --force:*)"
    ]
  }
}
```

Allow-listing your common safe commands cuts the prompt noise dramatically. Deny-listing the genuinely dangerous ones is a backstop. See [`examples/settings.json.example`](../examples/settings.json.example).

## Always keep a human in the loop for these

No matter how much you trust the tool, these deserve your eyes every time:

- **Deleting files or data** — especially anything you didn't create.
- **`git push --force`** and history rewrites.
- **Deploying to production.**
- **Rotating or touching secrets and credentials.**
- **Anything outward-facing** — sending email, posting to an API, publishing.
- **Irreversible infrastructure changes** — dropping databases, terminating servers.

The principle: reversible actions can flow; **hard-to-undo or outward-facing actions stop for confirmation.**

## Don't expose secrets

- Keep secrets in `.env` files and your secret manager, never in `CLAUDE.md` or committed config.
- Make sure `.env` is in `.gitignore` *before* the agent (or you) commits.
- If a secret does leak into history, rotate it — don't just delete the line.

## Sandboxing for bigger autonomy

If you want to give the agent a long leash safely, run it in an isolated environment — a container, a VM, or a disposable cloud sandbox — where mistakes can't reach anything important. High autonomy plus a real blast radius is the combination to avoid; high autonomy in a sandbox is fine.

→ [6. Skills & Slash Commands](06-skills-and-commands.md)
