---
name: my-skill
description: <What it does> Use when <the trigger situation, not a phrase list>.
---

# <Skill name>

<One sentence giving Claude its role for this skill — what it's helping the user do.>

## Workflow

### 1. <First step>
<Imperative instruction. If this step needs deep detail, defer it:>
Read `references/<topic>.md` before this step.

### 2. <Next step>
<...>

### 3. <Final step>
<...>

## Done when
<The observable condition that means the task is complete.>

<!--
Authoring checklist (delete before shipping):
- description: 1–2 sentences, capability + when-to-use, no trigger-phrase list, ≤ 1024 chars
- body: workflow only, under 500 lines / ~6 KB; push catalogs and rules into references/
- references/: one focused file per topic, loaded via inline pointers above
- allowed-tools (in plugin command/agent configs): scoped to what's actually used, never *
- run /skill-validate, then /context and /cost to confirm a small footprint
-->
