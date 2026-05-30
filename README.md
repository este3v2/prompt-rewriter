# prompt-rewriter

A Claude Code skill for knowledge workers who use Claude or ChatGPT daily and want more consistent results from the prompts they already have.

## The problem it solves

Most people build prompts the same way: they get a bad output, they add a line to fix it. Then another. Six months later, the prompt is 400 words and half of it is doing nothing — or worse, quietly limiting what the model can do.

Anthropic's own research shows modern AI models reason internally without needing step-by-step instructions. Keeping old scaffolding in your prompts doesn't guide the model — it constrains it.

This skill audits your prompt, removes what doesn't belong, and rewrites it into a format that works with how these models actually think.

## What to expect

- More consistent outputs from prompts you already use
- A clear diff showing what was kept, removed, and why
- A list of risk items — things removed that you should test before switching to the new version

What it does not promise: the rewritten prompt will always be better on the first try. The risk items section exists for a reason. Test on 3–5 real inputs before replacing your original.

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

## References

- Anthropic — [Prompt engineering overview](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/overview)
- Anthropic Research — [Tracing the thoughts of a large language model](https://www.anthropic.com/research/tracing-thoughts-language-model) (2024)

---

Part of the [AI Crew experiment series](https://substack.com/@estebanpinto).
