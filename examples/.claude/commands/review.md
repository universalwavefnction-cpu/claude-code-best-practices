---
description: Review the current diff for bugs and convention violations
---

Review the staged and unstaged changes in this repository.

For each changed file:
- Flag correctness bugs, edge cases, and error paths that aren't handled.
- Note anything that violates the conventions documented in CLAUDE.md.
- Point out secrets, debug statements, or files that shouldn't be committed.
- Suggest simplifications — but only high-confidence ones, not stylistic nitpicks.

Be concise. Group findings by severity (blocker / should-fix / nit).
Do not change any code — this is a review only.
