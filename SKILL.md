---
name: prompt-rewriter
description: Rewrite any system prompt by stripping compensating complexity — procedural scaffolding, role-play preambles, format micromanagement, and duct-tape instructions added for old model limits. Rewrites to a clean 4-component outcome-based format (Outcome / Constraints / Tools / Coordination). Shows before/after diff, complexity reduction percentage, risk items, and a live output comparison. Trigger on "rewrite this prompt", "simplify prompt", "strip scaffolding", "clean up my prompt", "outcome-based prompt", or when user pastes a prompt and asks what's wrong with it.
---

# Prompt Rewriter

```
  [bloated prompt]
        │
        ▼
  ask 2 questions
  ┌─────────────────────────────────────┐
  │ 1. sample input available?          │
  │ 2. deployment target?               │
  └─────────────────────────────────────┘
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
                │
                ▼
      live output diff (original vs rewritten)
```

> Built from real client sessions. The most common finding: the bloat isn't the problem — it's hiding an empty prompt. Use the risk items section to catch this before deploying.

## Why this matters before anything else

Most prompts were written for a 2022 model. Every line you added to fix a bad output is still running today — on a model that no longer needs it.

The cost is not aesthetic. It is operational:

- **Token cost** — every bloated line runs on every message, every session, every agent call. A 400-word system prompt that could be 100 words burns 300 tokens per turn at scale.
- **Reliability cost** — contradictory instructions (global CLAUDE.md vs project CLAUDE.md vs skill) produce inconsistent outputs. Leaner prompts have fewer collision points.
- **Maintenance cost** — bloated prompts grow. Every new bad output becomes a new duct-tape line. In 6 months you have a 600-word prompt nobody can read or reason about.

The most common finding after running this: **the bloat isn't the problem — it's hiding an empty prompt.** Strip the scaffolding and there is no outcome underneath. That is the real diagnosis.

---

## Where this hits hardest (ranked by impact)

**1. ClaudeClaw / Cowork agent system prompts** — highest impact
Every agent session loads the system prompt in full. A bloated SKILL.md or agent instruction burns tokens on every Telegram message, every session, every parallel run. This is where a 60% reduction in prompt length translates directly to lower cost and faster, more consistent responses. Run this here first.

**2. Claude Code SKILL.md files** — high impact
Skills are injected into context on every invocation. Bloated skills slow the harness and create instruction collisions with CLAUDE.md. A lean SKILL.md with a clear outcome runs faster, costs less, and is easier to maintain as the system evolves.

**3. CLAUDE.md project instructions** — high impact on consistency
CLAUDE.md loads at the start of every session. Contradictory or over-specified instructions here create compounding confusion — the model tries to satisfy conflicting rules and produces unpredictable outputs. Rewriting CLAUDE.md to outcomes-only removes the contradiction surface.

**4. Claude.ai Projects (system prompt)** — medium impact
Project system prompts load on every conversation. Lean format preferred — Claude.ai does not benefit from the procedural detail that some Claude Code skills tolerate. Strip all scaffolding; the model already knows how to structure outputs.

**5. Codex task descriptions** — low to medium impact
The 4-section format maps to Codex task structure but tooling and execution differ. Useful for cleaning up vague task descriptions; less useful for Codex-specific agent coordination patterns.

---

## When NOT to run this skill

- On a prompt that is already under 50 words and producing good output — leave it alone
- On a prompt where every line is load-bearing — the skill will confirm it, but there is nothing to cut
- On Codex coordination prompts with complex tool orchestration — the format does not map cleanly

---

## Step 0 — Gather context (before any analysis)

Before Phase 1, use AskUserQuestion to ask the user two things:

**Question 1:** "Do you have a sample input to test the prompt against?"
- Options: "Yes, I'll paste it" / "No, skip the live diff"
- If yes, collect the input — it will be used in Phase 5

**Question 2:** "Where is this prompt being deployed?"
- Options: Claude Code skill / ClaudeClaw or Cowork agent / Claude.ai Project / Codex / Other
- Use the answer to tailor Phase 2 — e.g. Claude Code skills tolerate more structure than Claude.ai Projects

If the user skips both, proceed to Phase 1 and note that Phase 5 will be omitted.

---

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

## Phase 5 — Live output diff

Only run this phase if the user provided a sample input in Step 0.

Run the **original prompt** against the sample input. Show the output under:
> **Original output:**

Run the **rewritten prompt** against the same input. Show the output under:
> **Rewritten output:**

Then add a 2–3 line summary:
> **What changed in the output:** [shorter / more specific / fewer hedges / different structure / etc.]

If the original prompt was too vague to produce a meaningful output, simulate what it likely would have produced based on the classification — label it clearly as simulated.

If no sample input was provided, end with:
> Phase 5 skipped — paste a sample input and re-run to see the live output diff.
