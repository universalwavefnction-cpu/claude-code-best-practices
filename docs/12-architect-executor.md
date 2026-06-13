# 12. Architect & Executor — Plan Before You Build

One of the most reliable ways to get good results on anything non-trivial is to split the work into two distinct modes:

- **Architect** — *think*. Investigate, design, produce a plan. Change nothing.
- **Executor** — *build*. Implement the approved plan.

The mistake most people make is collapsing these into one: "just go fix it." For a one-line change that's fine. For anything with moving parts, it means the agent commits to an approach — and writes code for it — before you've had a chance to catch a wrong assumption. By the time you see the problem, there's a diff to unwind.

Separating the modes makes the cheap moment — redirecting a *plan* — happen before the expensive moment — redirecting *code*.

## The architect pass

In architect mode the agent is **read-only**. It explores the codebase, asks clarifying questions, weighs trade-offs, and produces a concrete plan: which files it will touch, the approach, the order of operations, the risks.

Claude Code has this built in as **plan mode** — the agent researches and proposes but cannot edit until you approve. Enter it for any task where you're not 100% sure of the approach, or where the blast radius is large.

A good architect prompt asks for a plan, not a result:

> Before changing anything: how would you add rate limiting to the API? List the files you'd touch, the approach, and anything risky. Don't write code yet.

What you get back is reviewable in seconds. You can correct a wrong assumption ("no, use the existing middleware, don't add a library"), tighten scope, or kill a bad idea — all before a single line is written.

## The executor pass

Once the plan is right, switch to executor mode and let it build. Now the agent has an agreed blueprint, so it can move fast and you can give it more rope — accept-edits mode is often appropriate here, because the *what* is already settled and only the *how* remains.

> The plan looks good. Implement it. Run the tests when you're done and show me they pass.

Because the thinking already happened, execution is mechanical and far less likely to wander.

## Why the split works

- **Catch errors when they're cheap.** A flawed plan is three sentences to fix; flawed code is a diff to revert and redo.
- **You stay in control of direction without micromanaging.** You approve the *what*; the agent owns the *how*.
- **Cleaner results.** An agent following an agreed plan writes more coherent code than one discovering the design as it types.
- **Better use of context.** The architect pass front-loads understanding, so the executor isn't re-deriving the design mid-implementation.

## Variations

- **Same session, two phases.** The common case: plan mode → review → approve → build. One conversation, two gears.
- **Two agents.** For bigger work, one agent (or a planning subagent) produces the plan; another executes it. The planner can use a more capable model for the hard thinking, the executor a faster one for the mechanical work. See [Subagents & Workflows](08-subagents-workflows.md).
- **Fresh context for execution.** For a large plan, you can hand the approved plan to a clean session as the spec, so the executor starts with a focused context aimed only at building.

## When to skip it

Don't ceremony-up small work. A typo fix, a one-line config change, an obvious rename — just do it. The architect/executor split earns its keep when the task has real design decisions, touches multiple files, or is hard to undo. **Match the rigor to the stakes:** quick changes go direct; anything you'd want to review the *approach* of gets an architect pass first.

## The one-sentence version

Decide *what* and *how* before you write — plan in a read-only architect pass, review it cheaply, then switch to executor mode and build the thing you both agreed on.

← [Back to README](../README.md) · See also [3. Effective Prompting](03-effective-prompting.md) · [5. Permissions & Safety](05-permissions-safety.md)
