# 3. Effective Prompting

The quality of what you get back is mostly determined by the quality of what you ask. Claude Code is capable; the bottleneck is usually a vague request.

## Context first, then the ask

The strongest prompts follow a simple shape: **situation → goal → constraints**.

Weak:
> Fix the login bug.

Strong:
> The login form at `src/auth/login.tsx` rejects valid email addresses — users report that addresses with `+` in them fail. Find the validation logic causing this and fix it. Keep the existing error-message UX.

The second version tells the agent where to look, what "fixed" means, and what not to break. It will find the answer faster and get it right the first time.

## Be specific about "done"

Define success so the agent knows when to stop and how to check itself:

- "...and add a test that covers the `+` case."
- "...the fix passes `npm test` with no new failures."
- "...don't change the public API of the function."

Vague success criteria lead to under- or over-shooting. Concrete ones let the agent verify its own work.

## Let it act — don't dictate keystrokes

A common mistake is treating the agent like autocomplete: "open file X, go to line 40, change `foo` to `bar`." That throws away its main strength. Instead, describe the *outcome* and let it figure out the *how*:

> Rename the `foo` config flag to `bar` everywhere it's used, including docs and tests.

It will find all the call sites — including the ones you forgot.

## When the task is big, ask for a plan first

For anything substantial, ask the agent to plan before it writes:

> Before changing anything, outline how you'd add rate limiting to the API. List the files you'd touch and the approach.

Review the plan, correct course cheaply, *then* tell it to proceed. This catches wrong assumptions before they become wrong code. (Claude Code also has a dedicated **plan mode** — it researches and proposes without editing until you approve.)

## One task at a time

Don't bundle unrelated work into one prompt ("fix the login bug and also refactor the database layer and update the README"). Each task deserves a focused context. Do them in sequence, clearing context between unrelated ones. See [Context Management](04-context-management.md).

## Give feedback like a reviewer

When the result isn't right, say *why* — the agent learns within the session:

- "This works but it's not idiomatic — use the existing `Result` type instead of throwing."
- "You added a dependency; do it without one, we keep this module dependency-free."

Specific feedback redirects effectively. "No, that's wrong" without a reason just makes it guess again.

## Point at examples

The fastest way to get code in your style is to point at code already in your style:

> Add a new `/users` endpoint following the same pattern as the `/orders` endpoint in `src/routes/orders.ts`.

The agent reads the example and matches its structure, naming, and conventions.

→ [4. Context Management](04-context-management.md)
