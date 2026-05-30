# prompt-rewriter

A Claude Code skill that takes any system prompt you use today and rewrites it into a clean, outcome-based format built for modern AI models.

Most operators learned to write prompts in 2022–2023 when models needed heavy scaffolding. Claude 4 and GPT-4o don't need any of that. Keeping it in your prompts actively degrades output.

This skill strips the bloat and rewrites to a 4-component format: **Outcome / Constraints / Tools / Coordination**.

## Install

```bash
claude plugins marketplace add este3v2/prompt-rewriter
```

Then use `/prompt-rewriter` in any Claude Code session.

## What it does

1. **Classifies** every line of your prompt as outcome, constraint, scaffolding, or duct-tape
2. **Rewrites** to the 4-component format — keeps only what matters
3. **Shows a diff** — what was kept, deleted, reworded, and why
4. **Flags risk items** — things deleted that you should test before deploying

Typical result: 30–60% shorter prompt, more consistent output.

## Example prompts

**Quick rewrite:**
```
/prompt-rewriter

Here is a system prompt I use regularly. Rewrite it using the
4-component outcome-based format.

[PASTE YOUR PROMPT HERE]
```

**Full audit first (recommended):**
```
First, classify every line of this prompt as one of:
- outcome / constraint / scaffolding / duct-tape

Then run /prompt-rewriter on the result.

[PASTE YOUR PROMPT HERE]
```

## The 4-component format

| Section | What goes here |
|---|---|
| Outcome | What the system must achieve — measurable, specific |
| Constraints | Hard rules that cannot be violated |
| Tools | Available tools and when to use them — not how |
| Coordination | Multi-agent handoffs (write "N/A" if single-agent) |

Everything else gets cut.

---

Part of the [AI Crew experiment series](https://substack.com/@estebanpinto).
