# 8. Subagents & Workflows

As tasks grow, two patterns scale the agent past what a single linear conversation can do well: **subagents** (parallel, isolated helpers) and **multi-step orchestration**.

## Subagents

A subagent is a fresh agent the main agent spawns to handle a focused piece of work. It runs in its **own context** and returns only its conclusion. Two reasons this is powerful:

1. **Context isolation.** A subagent that reads twenty files to answer "where do we validate emails?" does all that reading in *its* context. Your main session gets back a two-line answer, not twenty files of clutter. This keeps the main thread sharp on long jobs.
2. **Parallelism.** Several subagents can work at once. Three independent investigations — "audit the auth code", "audit the DB layer", "audit the API routes" — run concurrently instead of one after another.

### When to reach for one

- **Broad search** across many files where you only need the finding ("find every call site of X").
- **Independent parallel work** — multiple unrelated investigations or edits that don't depend on each other.
- **Reading across a large codebase** to answer a question, where pulling everything into the main context would dilute it.

### When *not* to

- A quick lookup where you already know the file. Just read it; spawning a subagent has overhead.
- Tightly dependent steps where each needs the previous result — that's sequential work, not parallel.

You usually don't micromanage this — describe the goal and the agent decides when delegation helps. But knowing the pattern lets you ask for it: "spin up parallel agents to review each module."

## Specialized agents

Beyond general-purpose helpers, you can define **custom agent types** with their own instructions and tool access — a `code-reviewer` that only reads and critiques, a `test-writer` tuned for your test conventions, a read-only `explorer` for safe investigation. These are configured like skills and let you give different jobs different "personalities" and permissions.

## Multi-step orchestration

For work that's bigger than one task — a migration across fifty files, a broad audit, a "find then verify then fix" pipeline — you want structure, not a single long chat. The shape that works:

1. **Decompose** — break the job into independent units (per file, per module, per finding).
2. **Fan out** — run the units in parallel.
3. **Verify** — have a second pass check the first (an independent reviewer is much better at catching a bad finding than the agent that produced it).
4. **Synthesize** — collect the verified results into one coherent output.

A concrete example — a thorough code review:

> Review the changed files across several dimensions (bugs, performance, security) in parallel. For each finding, have a separate pass try to *refute* it. Keep only the findings that survive. Then summarize.

The "try to refute it" step matters: adversarial verification kills plausible-but-wrong findings that a single pass would have reported with confidence.

## Match effort to the task

Don't bring heavy orchestration to a light task. "Find any obvious bugs" wants a quick look. "Comprehensively audit this service before launch" earns the full decompose-fan-out-verify-synthesize treatment with multiple reviewers. Scale the machinery to the stakes.

→ [9. Git Workflow](09-git-workflow.md)
