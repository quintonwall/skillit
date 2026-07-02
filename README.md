# skillit 🍳

**Make your skills sizzle.**

`skillit` is a Claude Code plugin for **founders** who need to turn an idea into a working app fast, on a
tight budget. It teaches you to build skills — reusable leverage that speeds up your product work — and makes
sure they're *lean enough to run all day without eating your runway*. It teaches the structure, scaffolds it
for you, and scores what you built.

## Why

`skillit` helps you build skills that are lean enough to run constantly and fast enough to ship, test, and
throw away. It bakes in the discipline from Anthropic's
[prompting best practices](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/claude-prompting-best-practices)
and Quinton Wall's
[token-efficient skills post](https://www.quintonwall.com/writing/best-practices-to-make-claude-code-skills-token-efficient),
and makes you *measure* the result with the native `/context` and `/cost` commands. TL;DR: Less tokens spent = more runway for you to ship amazing products. 

## Installation

`skillit` is a **Claude Code** plugin. You cannot use it in the Claude Desktop app.

### From the marketplace (recommended)

Add this repo as a marketplace, then install the plugin from inside Claude Code:

```text
/plugin marketplace add quintonwall/skillit
/plugin install skillit@skillit
```

Update later with `/plugin marketplace update skillit`.

### Locally (for development)

**1. Clone the repo:**

```bash
git clone https://github.com/quintonwall/skillit.git
```

**2. Launch the Claude Code CLI pointed at the checkout.** Pass the plugin directory with `--plugin-dir`:

```bash
claude --plugin-dir /path/to/skillit
```

If you're already inside the cloned folder, `.` works too:

```bash
claude --plugin-dir .
```

A local `--plugin-dir` copy takes precedence over a same-named marketplace install for that session, so you
can safely test changes even if you also have `skillit` installed from the marketplace.

> There is **no** `/plugin-dir` slash command — local loading is the `--plugin-dir` launch flag only.


### Verify it loaded

Whichever route you used, confirm the plugin is live:

```text
/skillit:verify
```

If it loaded correctly, Claude recognizes the `skill-builder` skill plus the `/skillit:skill-new`,
`/skillit:skill-validate`, and `/skillit:skill-audit` commands, and you'll get a confirmation message. If nothing happens,
re-check the `--plugin-dir` path (or run `/reload-plugins`) and that you launched from the right directory.


## Quickstart

Just talk to Claude, and the `skill-builder` teacher walks you through it.
Here's a complete run, from idea to a shipped skill. (Numbers below are illustrative.)

> **Testing locally?** Launch with `claude --plugin-dir .` (see [Locally (for development)](#locally-for-development)),
> and run `/reload-plugins` after any edit to a skill, command, or hook so your changes take effect without
> restarting.

**1. Ask for what you want.**
```text
You:  Help me build a skill that drafts release notes from our git log.
```

**2. skillit baselines your cost, then asks the three things that shape a lean skill.**
```text
skillit:  Let's get you a lean, shippable one. Run /context first so we have a
          baseline to compare against. Then, three quick questions:
            • Capability — in one sentence, what does it do?
            • Trigger    — when should I reach for it?
            • Tools      — what does it actually need? (so we scope allowed-tools)

You:  /context
      → Skills: 1.2k tokens always-on

You:  It writes release notes from commits since the last tag. Use it when I'm
      cutting a release. It needs to run git and write a file.
```

**3. It scaffolds the 3-tier structure** (this is what `/skillit:skill-new` does):
```text
skillit:  Running /skillit:skill-new …
          Created skills/release-notes/
            ├── SKILL.md       ← frontmatter + workflow
            └── references/    ← deep detail, loaded only when needed
```

**4. It drafts the frontmatter with you — the highest-leverage part — and shows the trade-off.**
```text
skillit:  The description loads in every session, so tight wins.
            ✗  "Use when the user says 'cut a release', 'release notes',
                'changelog', 'tag a version', 'ship it'…"
            ✓  "Draft release notes from git commits since the last tag.
                Use when the user is cutting a release."
          The model routes on meaning, so the phrase list just burns tokens. OK?

You:  yes
```

**5. It builds the body with you**, keeping it to the workflow (find the last tag → collect commits →
group them → write the notes) and pushing anything bulky into `references/`.

**6. Validate and re-measure before shipping.**
```text
You:  /skillit:skill-validate
skillit:  Scorecard — release-notes
            Description quality ...... 2/2
            Body size & focus ........ 2/2
            Progressive disclosure ... 2/2
            Tool scoping ............. 2/2   (allowed-tools: Bash, Write)
            Frontmatter validity ..... 2/2
            Prompting craft .......... 2/2
            Grade: 12/12 — Sizzling 🍳

You:  /context   → Skills: 1.5k tokens   (+0.3k vs baseline)
You:  /cost      → this session: $0.04
```

That `+0.3k` always-on is the whole point: a skill lean enough to run all day without denting runway.
**You're done** — next time you say *"I'm cutting a release,"* the skill fires automatically.

## Optimize an existing skill (or all of them)

You don't have to start from an idea. If you already have skills — one you wrote, one you inherited, or a
whole folder of them — skillit will look at them and make them lean.

### One skill

Just ask. The `skill-builder` teacher triggers on "improve" and "optimize," so any of these work:

```text
You:  Look at my blog-writer skill and optimize it.
You:  This SKILL.md feels heavy — can you trim it?
```

Or point the scorer straight at it to get the graded scorecard and a prioritized fix list:

```text
You:  /skillit:skill-validate skills/blog-writer
skillit:  Scorecard — blog-writer
            Description quality ...... 1/2   (3 sentences, includes a trigger-phrase list)
            Body size & focus ........ 1/2   (210 lines — style guide belongs in references/)
            Progressive disclosure ... 0/2   (no references/; everything inline)
            Tool scoping ............. 2/2   (allowed-tools: Read, Write)
            Frontmatter validity ..... 2/2
            Prompting craft .......... 2/2
            Grade: 8/12 — Warming up
          Top fixes: 1) cut the phrase list from the description  2) move the
          style guide into references/style.md and link to it  3) …
```

Then hand the fixes back to the teacher — *"apply those fixes"* — and it edits the skill with you, one
change at a time, highest-leverage first (description → body → runtime). Re-run `/context` and `/cost` to
see the savings.

### Every skill in the project

To see all your skills at once — and the **total** always-on cost they add to every session — run the audit:

```text
You:  /skillit:skill-audit
skillit:  Skill          Grade   Always-on   Body    Top fix
          ────────────────────────────────────────────────────────────────
          blog-writer     8/12    412 chars   210 ln  Cut phrase list from description
          changelog       9/12    280 chars    90 ln  Move examples into references/
          release-notes  12/12    140 chars    42 ln  —
          ────────────────────────────────────────────────────────────────
          Combined always-on: ~832 chars across 3 skills.
          Biggest offender: blog-writer. Fix it first.
```

`skill-audit` globs every `SKILL.md` in the project, scores each one in parallel (off your main context),
and ranks them worst-first so you know where to spend your time. From there, just tell skillit which to fix:

```text
You:  Optimize blog-writer and changelog.
```

skillit walks each one through the same trim → validate → re-measure loop, so a whole folder of skills gets
lean without you scoring them by hand.

> **Structure, not output quality.** skillit's optimization is about token footprint and structure — *is it
> lean?* For *is the output still good?* after trimming, hand the skill to Anthropic's `/skill-creator` to
> benchmark behavior (see [below](#how-skillit-relates-to-the-official-skill-creator)).

## What's inside

| Component | What it does |
| --- | --- |
| `skill-builder` skill | A patient teacher that guides you idea → validated skill, one concept at a time, and answers questions as you go. Trigger it by asking to build, improve, or learn about a skill. |
| `/skillit:skill-new` | Scaffolds a best-practice 3-tier skill from your idea. |
| `/skillit:skill-validate` | Scores one skill against the rubric (via the `skill-scorer` subagent) and lists prioritized fixes. |
| `/skillit:skill-audit` | Scores **every** skill in your project at once and returns one ranked table, worst-offender first, with the combined always-on token footprint. |
| `skill-scorer` agent | Does the heavy scoring off your main context, then returns a compact scorecard. |
| `references/` | The durable knowledge: anatomy, frontmatter, token-optimization, prompting, rubric. |
| hook | A cheap nudge to run `/skillit:skill-validate` after you edit a `SKILL.md`. No model call. |

## How skillit relates to the official skill-creator

[`skill-creator`](https://claude.com/plugins/skill-creator) is Anthropic's lifecycle and **evaluation**
toolkit — Create / Eval / Improve / Benchmark, with agents that run a skill against eval prompts, grade its
outputs, A/B-compare versions, and benchmark with statistical confidence. It answers *"does this skill
produce good output, and which version wins?"*

`skillit` deliberately covers the two things skill-creator leaves open:

- **Teaching the *why*** — a patient, ask-anything tutor for someone learning to author skills, not a
  mode-driven power tool.
- **Token & structure efficiency** — skill-creator's scope doesn't include token optimization; that's
  skillit's whole point (`/context`, `/cost`, and the structural scorecard).

They're complementary, on different axes: skillit validates **cost and structure** (is it lean?),
skill-creator validates **behavior** (is the output good?).

**Handoff:** build and trim your skill with `skillit`, then reach for `/skill-creator` to benchmark its
output quality once it's structurally sound.

## Design principle

`skillit` measures before it optimizes and never duplicates state — the scoring rubric lives in exactly one
place (`references/rubric.md`), shared by the `/skillit:skill-validate` command and the `skill-scorer` agent.
