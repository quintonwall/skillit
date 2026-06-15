# Writing the frontmatter (the highest-leverage tokens you'll spend)

The `description` loads in **every session**, for every user, whether the skill runs or not. Reducing it is
the single highest-leverage optimization in a skill — a tighter description cuts always-on cost across all
sessions at once.

## Field rules

```yaml
---
name: my-skill            # ≤ 64 chars, lowercase / numbers / hyphens, no reserved words
description: <one or two tight sentences>   # ≤ 1024 chars, non-empty, no XML tags
---
```

## The description formula: capability + when-to-use

Write one or two tight sentences that say **what the skill does** and **when to reach for it** — described as
a *situation*, not a list of trigger phrases.

<example>
**Bloated (a trigger-phrase phrasebook):**
> Use this when the user says "run the tests", "run api tests", "test the api", "check the endpoints",
> "validate the api", "run postman", "execute the collection", or asks anything about testing the API.

**Tight (capability + when-to-use):**
> Run the project's API test collection. Use after code changes or when the user asks to test the API.
</example>

The model routes on meaning, not on matching exact phrases — so the phrasebook adds cost without adding
recall. One clear situation generalizes to all of those phrasings.

## Rules of thumb

- **One or two sentences.** If it runs to a paragraph, it's doing too much.
- **No phrasebooks.** Don't enumerate the ways a user might ask. Describe the situation once.
- **Tell what to do, not what not to do.** Positive framing is shorter and clearer.
- **Name the trigger situation explicitly.** "Use after X" or "Use when the user wants Y" beats leaving the
  model to infer relevance from a capability statement alone.
- **Don't restate the body.** The description is a router, not a summary.

A good description is the difference between a skill that triggers reliably for a few tokens and one that
either misfires or taxes every session. Treat it as expensive real estate.
