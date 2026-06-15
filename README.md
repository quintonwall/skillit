# skillit 🍳

**Make your skills sizzle.**

`skillit` is a Claude Code plugin for **founders** who need to turn an idea into a working app fast, on a
tight budget. It teaches you to build skills — reusable leverage that speeds up your product work — and makes
sure they're *lean enough to run all day without eating your runway*. It teaches the structure, scaffolds it
for you, and scores what you built.

It's built to be its own worked example: a slim `SKILL.md`, deep material deferred to `references/`, and
scoped tools. The plugin teaches token-efficient skills by being one.

## Why

You're short on cash, shorter on time, and hunting product-market fit. Tokens are dollars and dollars are
runway — and a bloated skill quietly taxes *every* session, because its frontmatter loads whether you use it
or not. Cutting that cost means more experiments per dollar: more shots on goal before the money runs out.

`skillit` helps you build skills that are lean enough to run constantly and fast enough to ship, test, and
throw away. It bakes in the discipline from Anthropic's
[prompting best practices](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/claude-prompting-best-practices)
and Quinton Wall's
[token-efficient skills post](https://www.quintonwall.com/writing/best-practices-to-make-claude-code-skills-token-efficient),
and makes you *measure* the result with the native `/context` and `/cost` commands.

## Installation

Run these steps inside Claude Code (the Claude app / Claude Code chat). The /plugin commands are Claude
slash-commands, so they must be entered in Claude, not in a normal terminal shell.

### From the marketplace (recommended)

Add this repo as a marketplace, then install the plugin from inside Claude Code:

```text
/plugin marketplace add quintonwall/skillit
/plugin install skillit@skillit
```

`quintonwall/skillit` is the GitHub `owner/repo` shorthand; a full `https://github.com/quintonwall/skillit.git`
URL works too. Update later with `/plugin marketplace update skillit`.

### Locally (for development)

Clone the repo and add it as a marketplace from its path in Claude Code:

```bash
git clone https://github.com/quintonwall/skillit.git
```

```text
/plugin marketplace add ./skillit
/plugin install skillit@skillit
```

To update this local copy later, run:

```text
/plugin marketplace update skillit
```

If you get an error such as “unknown command /plugin” or “command not found”, you are probably running the
lines in a normal shell instead of Claude Code. The `/plugin` commands only work in the Claude app / Claude
Code chat window. Open Claude, paste the two lines there, and then retry.

### Verify it loaded

After install, run the built-in verification command:

```text
/skillit verify
```

If the plugin loaded correctly, Claude should recognize the `skill-builder` skill and the `/skill-new` /
`/skill-validate` commands. 


## Quickstart

There's no wizard — you just talk to Claude, and the `skill-builder` teacher walks you through it.
Here's a complete run, from idea to a shipped skill. (Numbers below are illustrative.)

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

**3. It scaffolds the 3-tier structure** (this is what `/skill-new` does):
```text
skillit:  Running /skill-new …
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
You:  /skill-validate
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

## What's inside

| Component | What it does |
| --- | --- |
| `skill-builder` skill | A patient teacher that guides you idea → validated skill, one concept at a time, and answers questions as you go. Trigger it by asking to build, improve, or learn about a skill. |
| `/skill-new` | Scaffolds a best-practice 3-tier skill from your idea. |
| `/skill-validate` | Scores a skill against the rubric (via the `skill-scorer` subagent) and lists prioritized fixes. |
| `skill-scorer` agent | Does the heavy scoring off your main context, then returns a compact scorecard. |
| `references/` | The durable knowledge: anatomy, frontmatter, token-optimization, prompting, rubric. |
| hook | A cheap nudge to run `/skill-validate` after you edit a `SKILL.md`. No model call. |

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
place (`references/rubric.md`), shared by the `/skill-validate` command and the `skill-scorer` agent.
