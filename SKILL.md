---
name: prompt-rewriter
description: Rewrite any system prompt by stripping compensating complexity — procedural scaffolding, role-play preambles, format micromanagement, and duct-tape instructions added for old model limits. Rewrites to a clean 4-component outcome-based format (Outcome / Constraints / Tools / Coordination). Shows before/after diff, complexity reduction percentage, and risk items to test before deploying. Trigger on "rewrite this prompt", "simplify prompt", "strip scaffolding", "clean up my prompt", "outcome-based prompt", or when user pastes a prompt and asks what's wrong with it.
---

# Prompt Rewriter

```
  [bloated prompt]
        │
        ▼
  classify every line
  ┌─────────────┬──────────────┬─────────────┬──────────┐
  │  OUTCOME ✓  │ CONSTRAINT ✓ │ SCAFFOLDING │ DUCT-TAPE│
  └─────────────┴──────────────┴─────────────┴──────────┘
        │               │             │             │
       keep            keep         delete        delete
        │               │
        └───────┬────────┘
                ▼
      4-component rewrite
      ┌──────────────────────────┐
      │ ## Outcome               │
      │ ## Constraints           │
      │ ## Tools                 │
      │ ## Coordination          │
      └──────────────────────────┘
                │
                ▼
      before/after diff + risk items
```

> Built from real client sessions. The most common finding: the bloat isn't the problem — it's hiding an empty prompt. Use the risk items section to catch this before deploying.

## What is compensating complexity?

Bloat that accumulated for a 2022 model and now constrains a 2026 model:

- **Scaffolding** — step-by-step procedures the model already knows ("First read the input. Then identify themes. Then write a summary.")
- **Duct-tape** — instructions added after one bad output, constraining every output since ("Do NOT use bullet points unless asked.")
- **Format micromanagement** — over-specified structure for outputs the model formats well on its own
- **Redundant role-play** — "You are a world-class..." preambles that no longer change behaviour on capable models

## Phase 1 — Classify

Do a fast classification pass on every line of the input prompt.

Label each line as one of:
- `outcome` — defines what must be achieved
- `constraint` — a hard rule that cannot be violated
- `scaffolding` — step-by-step procedure compensating for old model limits
- `duct-tape` — added because something broke once, not because it's needed
- `risk` — vague or missing — cannot be kept or deleted without more information

Show the classification as a table: **Line | Type | Reason**

End the table with a summary line: `X duct-tape · X scaffolding · X outcomes · X constraints · X risk items`

## Phase 2 — Rewrite

From the classified lines, extract only what matters:

**Keep:**
- Outcomes (what must be achieved — measurable, specific)
- Constraints (hard rules — NOT things the model would do anyway)

**Discard:**
- All scaffolding
- All duct-tape
- Vague noise ("be thorough", "sound human", "consider all angles")

**Flag as risk items:**
- Anything deleted that could affect output in edge cases
- Anything too vague to keep but potentially load-bearing (e.g. "match my brand")

Produce a clean prompt in this exact 4-section format:

```
## Outcome
[What this system must achieve — measurable, no "how"]

## Constraints
[Hard rules only. If the model would do it anyway, it's not a constraint.]

## Tools
[Available tools and when to use them — not how. Write "None." if none.]

## Coordination
[Multi-agent handoffs only. Write "N/A" if single-agent.]
```

Rules:
- No imperative procedures ("First do X, then do Y")
- No role-play preambles
- Total length must be shorter than the original

## Phase 3 — Diff + risk assessment

Show a before/after comparison table:

| What | Original | Decision |
|---|---|---|
| Kept | [line] | [why] |
| Deleted | [line] | [why] |
| Reworded | [original → new] | [what changed] |

Then list all risk items explicitly:
> ⚠️ **[what was deleted]** — [why it might still matter] — test before deploying

## Phase 4 — Length comparison

| | Words | Lines |
|---|---|---|
| Original | X | X |
| Rewritten | X | X |
| Reduction | X% | X% |

If the rewritten prompt is longer than the original, stop and explain why — you added complexity instead of removing it.
