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
| ClaudeClaw / Cowork agent | Highest | Agent system prompts, Telegram skill flows, SKILL.md files |
| Claude Code | High | CLAUDE.md instructions, SKILL.md prompts, hook descriptions |
| Claude.ai Projects | Medium | Project system prompt, custom instructions |
| Codex | Partial | Task descriptions — 4-section format maps but tooling differs |

Run it on agent system prompts first. That is where a 60% reduction in prompt length translates directly to lower cost and faster, more consistent responses per session.

## Install

```bash
claude plugins marketplace add este3v2/prompt-rewriter
```

Then use `/prompt-rewriter` in any Claude Code session.

## How to use it

**Option 1 — Quick rewrite:**
```
/prompt-rewriter

Here is a prompt I use regularly. Rewrite it into a clean
outcome-based format. Show me the before/after diff and flag
anything I should test before using the new version.

[PASTE YOUR PROMPT HERE]
```

**Option 2 — Audit first (recommended if the prompt is long):**
```
First, classify every line of this prompt as one of:
- outcome: defines what the result should be
- constraint: a rule the model must follow
- scaffolding: step-by-step instructions the model doesn't need
- duct-tape: added after a bad output, constraining everything since

Then rewrite it keeping only outcomes and constraints.

[PASTE YOUR PROMPT HERE]
```

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

## References

- Anthropic — [Prompt engineering overview](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/overview)
- Anthropic Research — [Tracing the thoughts of a large language model](https://www.anthropic.com/research/tracing-thoughts-language-model) (2024)

---

Part of the [AI Crew experiment series](https://substack.com/@estebanpinto).
