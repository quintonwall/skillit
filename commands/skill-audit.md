---
description: Audit every skill in a project at once — score each against the skillit rubric and return one ranked table, worst-offender first, with the combined always-on token footprint.
argument-hint: [dir-to-scan]
allowed-tools: Task, Glob, Read
---

# /skillit:skill-audit

Score **all** skills in a project and return a single consolidated report, so a founder can see total
always-on burn and know which skill to fix first. Each skill is scored by the `skill-scorer` subagent so the
heavy reading stays off the main context.

## Steps

1. **Discover the skills.** Glob for every `SKILL.md` under the target directory: use `$ARGUMENTS` as the
   root if given, otherwise the current working directory. Search common locations
   (`**/skills/**/SKILL.md` and `**/SKILL.md`). De-duplicate the paths. If none are found, tell the user and
   stop. If exactly one is found, run the single-skill flow instead — suggest `/skillit:skill-validate` and
   score just that one.

2. **Score each skill in parallel.** Launch one `skill-scorer` agent (via the Task tool) **per skill**, all
   in a single message so they run concurrently. Give each the resolved path to its `SKILL.md`. Do not score
   inline — let the subagents do it so the main context stays clean.

3. **Consolidate into one ranked table.** From each returned scorecard, pull the grade, the measured facts,
   and the single highest-leverage fix. Build one table sorted **worst grade first** (ties broken by largest
   always-on description length — the most expensive per-session offender):

   ```
   Skill | Grade | Always-on (desc chars) | Body (lines) | allowed-tools | Top fix
   ```

4. **Report the combined footprint.** Below the table, sum the pieces that load on *every* session across
   all skills — the total description characters (always-on) — and state it plainly, e.g.
   "Combined always-on: ~N chars across M skills." This is the number that dents runway. Call out the one or
   two skills contributing most to it.

5. **Prompt the optimize loop.** Tell the user they can hand the worst offenders back to `skill-builder` to
   fix — e.g. *"optimize release-notes and blog-writer"* — or re-score any one with
   `/skillit:skill-validate <path>`. Then remind them to run `/context` and `/cost` to see the real footprint
   before and after. 🍳

Keep the report tight: one table plus the combined-footprint line. The per-skill detail already lives in each
subagent's scorecard — don't repeat all of it. Highest-leverage fixes first: description before body before
runtime.
