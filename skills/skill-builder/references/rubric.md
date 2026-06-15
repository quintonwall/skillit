# Skill scoring rubric

The single source of truth for `/skill-validate` and the `skill-scorer` agent. Both read this file; neither
restates it (eliminate duplicate state).

Score each dimension 0–2: **2** = meets the bar, **1** = partial / minor issue, **0** = fails. Sum to a grade
out of 12.

## Dimensions

### 1. Description quality (weight: highest)
- **2** — One or two sentences, capability + when-to-use, names a clear trigger situation, no phrasebook.
- **1** — Slightly long, or trigger situation is vague, or mild phrasebook tendencies.
- **0** — Paragraph-length, a list of trigger phrases, missing the when-to-use, or restates the body.
- *Check:* `description` length in characters; sentence count; presence of "when the user says…"-style lists.

### 2. Body size & focus
- **2** — Body under 500 lines / ~6 KB and contains the workflow only.
- **1** — Slightly over, or mixes some reference-grade detail into the body.
- **0** — Well over the limit, or the body is a rule catalog rather than a workflow.
- *Check:* line count and byte size of `SKILL.md`.

### 3. Progressive disclosure
- **2** — Bulk detail lives in `references/`, loaded via inline pointers at the step that needs it.
- **1** — Has `references/` but loads them all up front, or some bulk still inlined.
- **0** — No `references/`; everything crammed into the body.
- *Check:* existence of `references/`; whether the body says "read references/x.md".

### 4. Tool scoping
- **2** — `allowed-tools` lists only the tools used (or the skill needs none and omits it appropriately).
- **1** — Mostly scoped but includes a tool or two it doesn't use.
- **0** — `allowed-tools: *` or a broad wildcard.
- *Check:* presence of `*`; tools listed vs tools actually referenced.

### 5. Frontmatter validity
- **2** — `name` ≤ 64 chars lowercase/hyphen; `description` ≤ 1024 chars, non-empty, no XML tags.
- **1** — Valid but borderline (e.g. near a limit, awkward name).
- **0** — Violates a field rule.
- *Check:* field constraints from `frontmatter.md`.

### 6. Prompting craft
- **2** — Clear imperative steps, gives a role, uses examples in `<example>` tags where helpful, no over-prompting.
- **1** — Mostly clear but vague in places, or mild "CRITICAL/MUST" over-prompting.
- **0** — Ambiguous instructions, no role, or heavy shouting.
- *Check:* against `prompting.md`.

## Grade bands
- **11–12** — Sizzling. Ship it. 🍳
- **8–10** — Solid; address the flagged 1s and 0s.
- **4–7** — Needs work; rework the lowest dimensions first.
- **0–3** — Rebuild from the template.

## Output format (the scorecard)
The scorer returns:
1. A markdown table: dimension | score (0–2) | one-line reason.
2. Measured facts: description length (chars), body size (lines / KB), `allowed-tools` value.
3. Total grade (X/12) and band.
4. A prioritized fix list — highest-leverage first (description before body before runtime).
