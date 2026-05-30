# prompt-rewriter

A Claude Code skill that audits and rewrites system prompts — removing instructions that were written for 2022-era models and now constrain modern ones.

## Why this matters

Most operators wrote their prompts when models needed heavy scaffolding: step-by-step procedures, role-play preambles, retry logic, format micromanagement. Claude 4 and GPT-4o don't need any of that.

Anthropic's own research ("Tracing Thoughts", 2024) shows modern Claude performs sophisticated internal reasoning without being explicitly instructed to — it plans ahead, runs parallel computation, and uses multi-step inference autonomously. When you write step-by-step procedures into a prompt, you're not guiding the model. You're constraining a reasoning path it would have chosen better on its own.

This is what Anthropic calls "compensating complexity" — instructions added to compensate for model limitations that no longer exist.

## What the expected outcome is

**Primary: better, more consistent outputs.**

Removing procedural constraints lets the model pick the optimal reasoning path per input rather than following a fixed script. Clear outcomes + hard constraints produce predictable behavior. Vague procedural instructions produce variable behavior because each model run interprets the steps differently.

Anthropic's prompt engineering guidance is explicit: "Prefer general instructions over prescriptive steps. A prompt like 'think thoroughly' often produces better reasoning than a hand-written step-by-step plan. Claude's reasoning frequently exceeds what a human would prescribe." ([source](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/overview))

**Secondary: token cost reduction.**

Shorter prompts cost less per call. This is real — typical rewrites reduce prompt length 30–60% — but it's a byproduct of removing instructions that weren't doing useful work, not the goal itself.

**What this is not:**

Shorter is not always better. Anthropic recommends examples, XML structure, explicit context, and precise success criteria — all of which can make prompts longer. The target is removing the *right* things (scaffolding, duct-tape), not minimizing word count.

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

## References

- Anthropic — [Prompt engineering overview](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/overview)
- Anthropic Research — [Tracing the thoughts of a large language model](https://www.anthropic.com/research/tracing-thoughts-language-model) (2024)

---

Part of the [AI Crew experiment series](https://substack.com/@estebanpinto).
