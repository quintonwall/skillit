# Token optimization: measure first, then apply the five techniques

Adapted from Quinton Wall's *Best practices to make Claude Code skills token-efficient*.

## Why a founder should care

Tokens are dollars, and dollars are runway. A skill that costs less to run lets you experiment more times
before the money runs out — more iterations toward product-market fit per dollar of burn. The frontmatter
tax is the sneakiest: it's charged in *every* session whether the skill fires or not, so a bloated
description quietly raises the cost of all your work. Lean skills aren't tidiness; they're more shots on goal.

## Measure before optimizing

You can't optimize what you don't measure. Use the native commands:

- **`/context`** — shows the always-on footprint: how many tokens the system prompt, skill frontmatter, and
  tool schemas consume before any work happens. Run it *before* and *after* a change to see the delta.
- **`/cost`** — shows real token and dollar spend for the session, including runtime costs like tool output.

## The cost hierarchy (optimize in this order)

1. **Always-on** — frontmatter descriptions and tool schemas. Loaded every session. **Highest leverage.**
2. **Per-trigger** — the SKILL.md body. Loaded only when the skill is relevant. Medium leverage.
3. **Runtime** — tool output, polling chatter, generated tokens. Loaded only while working. Lowest leverage.

Spend your effort top-down: a sentence trimmed from a description pays off in every session; a paragraph
trimmed from the body pays off only when the skill fires.

## The five techniques

### 1. Tighten frontmatter descriptions (highest priority)
Reduce to one or two sentences structured as capability + when-to-use. Delete trigger-phrase phrasebooks.
Typical saving: ~20% of always-on cost, across all sessions. See `frontmatter.md`.

### 2. Progressive disclosure via references
Keep the SKILL.md body small (~6 KB / under 500 lines). Move detailed rules, templates, and catalogs into
`references/`, and load them with inline pointers ("read `references/x.md` before this step"). Typical
saving: 32–60% of per-trigger cost — without deleting any content.

### 3. Eliminate duplicate state
Delete anything that restates what the harness already knows. If you're maintaining a routing table that
just repeats your frontmatter descriptions, delete the table — native routing already does that job. **Red
flag:** the same fact written in two places.

### 4. Scope tool access precisely
Replace wildcard `allowed-tools: *` with an explicit list of only the tools the component calls. This can
cut tool-schema tokens by an order of magnitude (e.g. 111 tools → 11) and surfaces latent permission bugs
that wildcards hide.

### 5. Back off async polling
For background jobs, poll with exponential backoff (2s → 4s → 8s) and report only the final outcome, not
every poll cycle. Narrating each poll bloats runtime context for no benefit.

## What good looks like

A full optimization pass on a real plugin achieved ~27% lower cost per session, ~46% faster wall time, ~53%
fewer tokens generated, and 2.7× better cache read-to-write efficiency — all from structure, not from
cutting features.

## Quick do / don't

**Do:** treat descriptions as expensive real estate; defer bulk content to on-demand references; audit
`allowed-tools` in review.

**Don't:** keep trigger-phrase lists; keep rule catalogs in the body; ship wildcard tool permissions.
