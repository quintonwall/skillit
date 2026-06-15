# Prompting craft for skill bodies

A SKILL.md body is a prompt. These are the techniques from Anthropic's prompting best practices that matter
most when writing one. The goal: instructions a new employee with no context could follow without confusion.

## Be clear and direct
State exactly what you want. If you want above-and-beyond behavior, ask for it — don't rely on the model to
infer it. Use numbered steps when order or completeness matters (a workflow almost always does).

## Add context / motivation
Explaining *why* an instruction exists helps the model generalize correctly.

<example>
Weak: "Never load all reference files at once."
Strong: "Load reference files only at the step that needs them — loading them all up front defeats
progressive disclosure and inflates context cost."
</example>

## Use examples (few-shot)
Examples steer format and tone more reliably than description alone. Make them relevant, diverse (cover edge
cases), and wrap each in `<example>` tags so the model distinguishes them from instructions. Three to five is
a good target.

## Structure with XML tags
When a prompt mixes instructions, context, and examples, wrap each kind in its own tag (`<example>`,
`<context>`, `<input>`). Consistent, descriptive tag names reduce misinterpretation.

## Give a role
Open the body by telling Claude what role it's playing ("Guide the user from idea to a token-efficient
skill…"). Even one sentence focuses behavior and tone.

## Tell what to do, not what not to do
"Write in flowing prose paragraphs" beats "don't use bullet points." Positive instructions are shorter and
the model follows them more reliably.

## Be explicit about action vs. suggestion
"Change this function" makes the model act; "can you suggest changes" makes it only advise. In a skill that
should *do* work, phrase steps as imperatives.

## Don't over-prompt
Modern models trigger tools and skills appropriately without shouting. Prefer "Use this when…" over
"CRITICAL: you MUST always use this." Aggressive language causes over-triggering and wastes tokens.

## Match prompt style to desired output
If you want concise prose output, write the skill body in concise prose. The formatting style of the prompt
influences the formatting of the response.
