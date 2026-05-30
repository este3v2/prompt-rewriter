# Prompt Rewriter — Free Prompts

These prompts work in Claude Code and Claude.ai. No paid account required.

---

## Which version are you using?

| Platform | How to use |
|---|---|
| **Claude Code (CLI)** | Install the skill (below), then use `/prompt-rewriter` |
| **Claude Desktop** | Install via the Skills panel (same SKILL.md format) |
| **Claude.ai (web)** | Skip the install — copy any prompt below and paste it directly into your chat |

---

## Install (Claude Code and Claude Desktop)

```bash
claude plugins marketplace add este3v2/prompt-rewriter
```

Then use `/prompt-rewriter` in any session.

---

## Prompt 1 — Quick rewrite

Paste this with your prompt attached.

```
Here is a system prompt I use regularly. Rewrite it using the
4-component outcome-based format (Outcome / Constraints / Tools / Coordination).

Strip all compensating complexity — step-by-step procedures,
role-play preambles, retry logic, format micromanagement,
and few-shot examples for things the model already knows.

Show me:
1. The rewritten prompt
2. A before/after diff (what was kept, deleted, reworded)
3. Risk items — anything deleted that I should test before deploying
4. Word count comparison (original vs. rewritten)

[PASTE YOUR PROMPT HERE]
```

---

## Prompt 2 — Full audit then rewrite (recommended)

Run this when you want to understand WHY the prompt is bloated before fixing it.

```
Classify every line of this prompt as one of:
- outcome: defines what the system must achieve
- constraint: a hard rule that cannot be violated
- scaffolding: step-by-step instructions that compensate for old model limits
- duct-tape: added because something broke, not because it's needed

Then rewrite it keeping only outcomes and constraints, using this format:

## Outcome
[what must be achieved — measurable, no "how"]

## Constraints
[hard rules only — if the model would do it anyway, it's not a constraint]

## Tools
[available tools and when to use them — write "None." if none]

## Coordination
[multi-agent handoffs only — write "N/A" if single-agent]

Show the before/after diff and flag anything I should test before replacing the original.

[PASTE YOUR PROMPT HERE]
```

---

## Prompt 3 — Write new prompts correctly from the start

Use this when starting a new workflow — skips the audit entirely.

```
Help me write a new prompt using the 4-component outcome-based format.

I want Claude to: [DESCRIBE WHAT YOU WANT]
Hard rules it must follow: [LIST ANY NON-NEGOTIABLE CONSTRAINTS]
Tools it has access to: [LIST TOOLS IF ANY]
Multi-agent handoffs needed: [YES/NO — describe if yes]

Write it clean. No procedural steps. No role-play preamble.
Total length should be under 150 words.
```

---

## The framework

**Four types of bloat to strip:**
- **Scaffolding** — step-by-step procedures the model already knows
- **Duct-tape** — added after one bad output, constraining every output since
- **Format micromanagement** — over-specified structure the model handles well on its own
- **Redundant role-play** — "You are a world-class..." preambles that add tokens, not expertise

**The only four things that belong in a prompt:**
- What must be achieved (Outcome)
- What cannot be violated (Constraints)
- What tools are available (Tools)
- Who to hand off to and when (Coordination — multi-agent only)
