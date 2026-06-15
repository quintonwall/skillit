# Skill anatomy & progressive disclosure

A Claude Code skill is a directory whose entry point is `SKILL.md`. Its power comes from **progressive
disclosure**: information loads in three tiers, so the agent pays for depth only when it needs it.

## The three tiers

1. **Frontmatter (always-on).** The YAML `name` and `description` are read at session startup for *every*
   skill. This is the most expensive real estate in the whole skill — it costs tokens in every session
   whether or not the skill is ever used. Keep it tight.
2. **Body (on-trigger).** The Markdown body of `SKILL.md` loads only when Claude decides the skill is
   relevant to the current task. It should hold the *workflow* — the steps to follow — and nothing else.
3. **References (on-demand).** Files under `references/` load only when a step explicitly tells Claude to
   read them. This is where bulk rules, catalogs, templates, and examples live.

The body should *point* to references at the moment they are needed ("read `references/frontmatter.md`
before this step"), not inline their contents.

## Directory layout

```
my-skill/
├── SKILL.md            # entry point: frontmatter + workflow body
├── references/         # docs loaded into context on demand
│   └── *.md
├── scripts/            # executable Python/Bash the skill can run
└── assets/             # templates, binaries, and other static files
```

- `references/` — knowledge the model reads. Split by topic so each load is small and targeted.
- `scripts/` — code the model executes rather than reads. Deterministic work (parsing, formatting,
  measuring) belongs here, not in prose.
- `assets/` — templates and binary files copied or referenced, not read into context.

## Size limits

- Keep the body **under 500 lines** (roughly **1,500–2,000 words**, ~6 KB). If it grows past that, the
  content almost certainly belongs in `references/`.
- There is no hard cap on `references/` total size, because those files are not loaded unless needed — but
  keep each individual file focused so a single load stays cheap.

## Frontmatter field rules

- `name`: ≤ 64 characters, lowercase letters / numbers / hyphens only. No XML tags, no reserved words.
- `description`: ≤ 1,024 characters, non-empty, no XML tags.

See `frontmatter.md` for how to write a description that earns its always-on cost.
