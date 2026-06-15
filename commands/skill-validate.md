---
description: Score a Claude Code skill's token efficiency and structure, returning a graded scorecard with prioritized fixes.
argument-hint: [path-to-skill-dir-or-SKILL.md]
allowed-tools: Task, Read
---

# /skillit:skill-validate

Score a skill against the `skillit` rubric and report a graded scorecard. The heavy reading and scoring runs
in a subagent so it stays off the main context — itself a token-efficiency demonstration.

## Steps

1. **Resolve the target.** Use `$ARGUMENTS` as the skill path if given; otherwise default to a `SKILL.md`
   in the current working directory. If none is found, ask the user for the path.

2. **Delegate to the scorer.** Launch the `skill-scorer` agent (via the Task tool) with the resolved path.
   The agent reads the rubric, the target `SKILL.md`, and its `references/`, then returns a structured
   scorecard. Do not score inline — let the subagent do it so the main context stays clean.

3. **Present the scorecard** exactly as returned: the per-dimension table, the measured facts (description
   length, body size, `allowed-tools`), the total grade (X/12) and band, and the prioritized fix list.

4. **Prompt the measurement loop.** Remind the user to run `/context` (compare to their baseline) and
   `/cost` to see the real footprint and spend — the rubric scores structure, these commands measure the
   actual tokens. 🍳

Address the highest-leverage fixes first: description before body before runtime.
