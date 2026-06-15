---
description: Scaffold a new token-efficient Claude Code skill from an idea, with the best-practice 3-tier structure.
argument-hint: [skill-name]
allowed-tools: Read, Write, AskUserQuestion
---

# /skill-new

Scaffold a new skill directory following the `skillit` best-practice structure. Bias toward doing the work —
create the files, don't just describe them.

## Steps

1. **Gather the essentials.** If `$ARGUMENTS` already names the skill, use it. Then ask the user (via
   AskUserQuestion, batching the questions) for anything missing:
   - **Capability** — what the skill does, in one sentence.
   - **Trigger** — the situation when Claude should use it (not a list of phrases).
   - **Tools** — which tools it actually needs (to scope `allowed-tools`).
   - **Location** — where to create it (default: `skills/<name>/` under the current project).

2. **Validate the name.** Lowercase, numbers, hyphens only, ≤ 64 chars. Fix it with the user if needed.

3. **Read the template.** Read `skills/skill-builder/assets/templates/SKILL.template.md` (and
   `reference.template.md`) from this plugin to use as the basis.

4. **Create the structure:**
   ```
   <location>/<name>/
   ├── SKILL.md            # from the template, with frontmatter filled in
   └── references/
       └── .gitkeep        # ready for on-demand detail files
   ```
   Fill the `SKILL.md` frontmatter from the answers: a `description` written as **capability + when-to-use**
   (one or two sentences, no phrasebook), and the validated `name`. Leave the body as a workflow skeleton.

5. **Report.** Show the created paths and tell the user the next steps: write the body, then run
   `/skill-validate`, then `/context` and `/cost` to confirm a small footprint. 🍳

Do not pad the SKILL.md with rules or catalogs — those go in `references/` later, loaded on demand.
