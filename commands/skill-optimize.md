---
description: Score a skill (or every skill in the project) and apply the fixes directly — tighten the description, enforce progressive disclosure, scope allowed-tools — then report a diff and the re-measured footprint. Applies changes instead of teaching.
argument-hint: [path-to-skill | dir | --all]
allowed-tools: Task, Glob, Read, Edit, Write, Bash
---

# /skillit:skill-optimize

Make a skill lean **without the tutorial**. This is the do-it-for-me counterpart to `skill-builder`: it
scores, applies the highest-leverage fixes directly to the files, and shows you what changed. Use it when you
already know you want the skill trimmed and just want it done.

> Edits happen in place. Assume the user has git (skillit is a repo tool) — the diff *is* the safety net.
> Do not create backups or branches unless asked.

## Resolve the target

- `$ARGUMENTS` is a skill path (dir or `SKILL.md`) → optimize that one skill.
- `$ARGUMENTS` is `--all`, a directory, or empty → glob every `SKILL.md` under the project
  (`**/skills/**/SKILL.md` and `**/SKILL.md`), de-duplicate, and optimize each. If more than a handful are
  found, list them and confirm before editing.
- Nothing found → say so and stop.

## Per skill

1. **Score first.** Launch the `skill-scorer` agent (Task tool) on the skill to get the graded scorecard and
   the prioritized fix list. When optimizing multiple skills, launch the scorers in parallel (one message).
   This is the baseline — capture the description length and body size before touching anything.

2. **Apply the fixes, highest-leverage first.** Only touch what scored below 2; leave a clean dimension
   alone. Draw the *how* from the plugin references, reading each only when a fix needs it:
   - **Description** (`references/frontmatter.md`) — rewrite to capability + when-to-use, one or two
     sentences. Strip trigger-phrase phrasebooks. This is the always-on cost, so do it first.
   - **Progressive disclosure** (`references/anatomy.md`) — move bulk detail (catalogs, style guides, long
     examples) out of `SKILL.md` into `references/<topic>.md`, and leave an inline pointer
     ("read `references/…`"). Create the `references/` dir if missing.
   - **Body** (`references/prompting.md`) — cut to the workflow: clear imperative steps, a role if it helps,
     examples in `<example>` tags. Delete duplicated state.
   - **Tool scoping** (`references/token-optimization.md`) — set `allowed-tools` to what the body actually
     uses; never leave `*`.
   Preserve the skill's behavior and voice — trim and restructure, don't rewrite what it does.

3. **Re-measure.** After editing, recompute the description length and `SKILL.md` size (`wc -c`, `wc -l`) so
   you can report the real before/after.

## Report (no lecture)

For each skill, a compact block — not a walkthrough:

```
skill-name
  Grade      8/12 → 12/12
  Always-on  412 → 140 chars   (-272)
  Body       210 → 44 lines    (moved style guide → references/style.md)
  Changes    • rewrote description (dropped 9-phrase trigger list)
             • extracted style guide + examples to references/
             • scoped allowed-tools: Read, Write
```

When optimizing several, end with the combined always-on before → after across all skills — the number that
frees up runway. Then tell the user to review the diff (`git diff`) and run `/context` and `/cost` to confirm
the savings. If they want the reasoning behind any change, they can ask `skill-builder`. 🍳

State what you changed plainly. If a skill was already lean (all 2s), say so and leave it untouched.
