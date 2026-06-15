# skillit 🍳

**Make your skills sizzle.**

`skillit` is a Claude Code plugin for **founders** who need to turn an idea into a working app fast, on a
tight budget. It teaches you to build skills — reusable leverage that speeds up your product work — and makes
sure they're *lean enough to run all day without eating your runway*. It teaches the structure, scaffolds it
for you, and scores what you built.

It's built to be its own worked example: a slim `SKILL.md`, deep material deferred to `references/`, and
scoped tools. The plugin teaches token-efficient skills by being one.

## Why

`skillit` helps you build skills that are lean enough to run constantly and fast enough to ship, test, and
throw away. It bakes in the discipline from Anthropic's
[prompting best practices](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/claude-prompting-best-practices)
and Quinton Wall's
[token-efficient skills post](https://www.quintonwall.com/writing/best-practices-to-make-claude-code-skills-token-efficient),
and makes you *measure* the result with the native `/context` and `/cost` commands.

## Installation

`skillit` is a **Claude Code** plugin. Run the `/plugin` and `/skillit:*` steps below inside a Claude Code
session (the CLI, or the Claude Code IDE integration) — *not* the standalone Claude desktop chat app, which
doesn't support Claude Code plugins. The shell commands (`git clone`, `claude --plugin-dir …`) run in your
terminal.

### From the marketplace (recommended)

Add this repo as a marketplace, then install the plugin from inside Claude Code:

```text
/plugin marketplace add quintonwall/skillit
/plugin install skillit@skillit
```

`quintonwall/skillit` is the GitHub `owner/repo` shorthand; a full `https://github.com/quintonwall/skillit.git`
URL works too. Update later with `/plugin marketplace update skillit`.

### Locally (for development)

When you're hacking on `skillit` itself, load the checkout directly with the `--plugin-dir` flag instead of
the marketplace install flow. This is the fastest dev loop and avoids the marketplace-registration issues
that can crop up with local installs.

**1. Clone the repo:**

```bash
git clone https://github.com/quintonwall/skillit.git
```

**2. Launch the Claude Code CLI pointed at the checkout.** Pass the plugin directory with `--plugin-dir`
(repeat the flag to load more than one plugin):

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

**3. Iterate without restarting.** After editing any plugin file (skills, commands, agents, hooks), reload
in-session:

```text
/reload-plugins
```

This re-reads skills, agents, hooks, commands, and plugin MCP/LSP servers. (If the plugin exposes MCP
servers with non-deferred tools, Claude will ask you to confirm with `/reload-plugins --force`, since
reloading invalidates the prompt cache.)

If you'd rather install it like a normal user, see [From the marketplace (recommended)](#from-the-marketplace-recommended)
above.

### Verify it loaded

Whichever route you used, confirm the plugin is live:

```text
/skillit:verify
```

Plugin commands are namespaced with the plugin name, so you type `/skillit:verify` (and `/skillit:skill-new`,
`/skillit:skill-validate`) — the `skillit:` prefix is required, not just a display label.

If it loaded correctly, Claude recognizes the `skill-builder` skill plus the `/skillit:skill-new` and
`/skillit:skill-validate` commands, and you'll get a witty "sizzling" confirmation. If nothing happens,
re-check the `--plugin-dir` path (or run `/reload-plugins`) and that you launched from the right directory.


## Quickstart

There's no wizard — you just talk to Claude, and the `skill-builder` teacher walks you through it.
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

## What's inside

| Component | What it does |
| --- | --- |
| `skill-builder` skill | A patient teacher that guides you idea → validated skill, one concept at a time, and answers questions as you go. Trigger it by asking to build, improve, or learn about a skill. |
| `/skillit:skill-new` | Scaffolds a best-practice 3-tier skill from your idea. |
| `/skillit:skill-validate` | Scores a skill against the rubric (via the `skill-scorer` subagent) and lists prioritized fixes. |
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
