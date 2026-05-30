# prompt-rewriter

A Claude Code skill for knowledge workers who use Claude or ChatGPT daily and want more consistent results from the prompts they already have.

## The problem it solves

Most people build prompts the same way: they get a bad output, they add a line to fix it. Then another. Six months later, the prompt is 400 words and half of it is doing nothing — or worse, quietly limiting what the model can do.

The cost is not aesthetic. It is operational:

- **Token cost** — every bloated line runs on every message, every session, every agent call. A 400-word system prompt that could be 100 words burns 300 tokens per turn at scale.
- **Reliability cost** — contradictory instructions produce inconsistent outputs. Leaner prompts have fewer collision points.
- **Maintenance cost** — bloated prompts grow. Every new bad output becomes a new duct-tape line. In 6 months you have a prompt nobody can read or reason about.

The most common finding: **the bloat isn't the problem — it's hiding an empty prompt.** Strip the scaffolding and there is no real outcome underneath. That is the real diagnosis.

## Where this hits hardest

| Context | Impact | What to rewrite |
|---|---|---|
| Claude Code | Highest | CLAUDE.md instructions, SKILL.md prompts, agent system prompts |
| Claude.ai Projects | Medium | Project system prompt, custom instructions |
| Codex | Partial | Task descriptions — 4-section format maps but tooling differs |

Run it on agent system prompts first. That is where a 60% reduction in prompt length translates directly to lower cost and faster, more consistent responses per session.

## Install

```bash
claude plugins marketplace add este3v2/prompt-rewriter
```

Then use `/prompt-rewriter` in any Claude Code session.

## How to use it

**Claude Code:** install the skill above, then:
```
/prompt-rewriter
[paste your prompt]
```

**Claude.ai (no install needed):** copy any prompt from [`free/prompts.md`](free/prompts.md) and paste it directly into your chat. All prompts work without the skill installed.

Full prompt library (quick rewrite, full audit, write-from-scratch) is in [`free/prompts.md`](free/prompts.md).

## The format it rewrites into

| Section | What goes here |
|---|---|
| Outcome | What the result must be — specific and measurable |
| Constraints | Rules that cannot be broken |
| Tools | Tools available and when to use them (write "None" if none) |
| Coordination | Handoffs to other agents (write "N/A" if not applicable) |

Everything else gets cut.

## Case study

**Original** — Claude.ai Project system prompt for a content assistant. 318 words. Written over 6 months by adding one fix at a time.

> You are an expert content strategist and writer... When I give you a topic, follow this process: 1. Read through everything carefully and identify the core message. 2. Think about who the target audience is. 3. Identify the best format. 4. Draft the content following the guidelines below. 5. Review your draft before sending... Do NOT sound corporate or formal. Make sure it doesn't sound like AI wrote it. Be thorough and consider all angles. Sound authentic and human. Always proofread before responding. Do NOT make things up...

Classification result: `7 duct-tape · 10 scaffolding · 4 outcomes · 10 constraints`

---

**Rewritten** — 97 words. Same outcomes and constraints. Everything else removed.

```
## Outcome
Write LinkedIn posts and Substack articles from topics, ideas, or rough notes.
When given rough notes, clean the language but preserve the user's voice.
Every piece opens with a hook. LinkedIn posts end with a question or reframe.
Substack articles end with a clear takeaway or call to action.

## Constraints
LinkedIn: 150–300 words · first person · no bullet points in body ·
no hashtags unless asked · banned words: delve, game-changer, unlock,
leverage, seamlessly · end with question or reframe

Substack: 600–1,200 words · headers required · no technical jargon ·
personal story only if the user provides one

Both: ask before writing if anything is unclear

## Tools
None.

## Coordination
N/A
```

**Reduction: 70% fewer words.**

---

**What changed in the output:**

The original prompt narrated its own process in every response — the scaffolding became visible text. The banned word "unlock" slipped through because 30 rules dilute attention. The rewritten prompt has one banned-word constraint, clearly scoped. No internal narration. Forbidden words stay forbidden.

## What it does not promise

The rewritten prompt will not always be better on the first try. The risk items section exists for a reason. Test on 3–5 real inputs before replacing your original.

## Grounded in Anthropic's own guidance

This skill applies Anthropic's prompt engineering best practices directly.

Anthropic's documentation for Claude 4.x states that modern models calibrate response length and complexity automatically, and reason internally without needing step-by-step procedures. Their recommendation: specify outcomes and constraints — not how to achieve them.

That is exactly what this skill does. Every line classified as scaffolding or duct-tape is a line that constrains a model Anthropic designed to reason without it.

**References:**
- Anthropic — [Prompting best practices for Claude's latest models](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/claude-prompting-best-practices)
- Anthropic — [Prompt engineering overview](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/overview)
- Anthropic Research — [Tracing the thoughts of a large language model](https://www.anthropic.com/research/tracing-thoughts-language-model) (2024)

---

Part of the [AI Crew experiment series](https://substack.com/@estebanpinto).
