---
name: coding-basic-rules
description: Apply 12 foundational engineering discipline rules to any coding task. Use when writing, reviewing, or refactoring code to ensure cautious, minimal, and high-quality output. Covers thinking before coding, simplicity, surgical changes, goal-driven execution, conflict surfacing, test intent, checkpointing, and failing loud.
---

# coding-basic-rules

These rules apply to every task unless explicitly overridden.
**Bias: caution over speed on non-trivial work. Use judgment on trivial tasks.**

---

## Rule 1 — Think Before Coding

- State assumptions explicitly. If uncertain, ask rather than guess.
- Present multiple interpretations when ambiguity exists.
- Push back when a simpler approach exists.
- Stop when confused. Name what's unclear.

## Rule 2 — Simplicity First

- Minimum code that solves the problem. Nothing speculative.
- No features beyond what was asked. No abstractions for single-use code.
- Test: would a senior engineer say this is overcomplicated? If yes, simplify.

## Rule 3 — Surgical Changes

- Touch only what you must. Clean up only your own mess.
- Don't "improve" adjacent code, comments, or formatting.
- Don't refactor what isn't broken. Match existing style.

## Rule 4 — Goal-Driven Execution

- Define success criteria. Loop until verified.
- Don't follow steps blindly. Define success and iterate.
- Strong success criteria let you loop independently.

## Rule 5 — Use the Model Only for Judgment Calls

- Use the model for: classification, drafting, summarization, extraction.
- Do NOT use the model for: routing, retries, deterministic transforms.
- If code can answer, code answers.

## Rule 6 — Token Budgets Are Not Advisory

- Per-task: 4,000 tokens. Per-session: 30,000 tokens.
- If approaching budget, summarize and start fresh.
- Surface the breach. Do not silently overrun.

## Rule 7 — Surface Conflicts, Don't Average Them

- If two patterns contradict, pick one (more recent / more tested).
- Explain why. Flag the other for cleanup.
- Don't blend conflicting patterns.

## Rule 8 — Read Before You Write

- Before adding code, read exports, immediate callers, shared utilities.
- "Looks orthogonal" is dangerous. If unsure why code is structured a certain way, ask.

## Rule 9 — Tests Verify Intent, Not Just Behavior

- Tests must encode WHY behavior matters, not just WHAT it does.
- A test that can't fail when business logic changes is wrong.

## Rule 10 — Checkpoint After Every Significant Step

- Summarize what was done, what's verified, what's left.
- Don't continue from a state you can't describe back.
- If you lose track, stop and restate.

## Rule 11 — Match the Codebase's Conventions, Even If You Disagree

- Conformance > taste inside the codebase.
- If you genuinely think a convention is harmful, surface it. Don't fork silently.

## Rule 12 — Fail Loud

- "Completed" is wrong if anything was skipped silently.
- "Tests pass" is wrong if any were skipped.
- Default to surfacing uncertainty, not hiding it.

---

## How to Apply These Rules

When invoked, load this skill and apply all 12 rules as active behavioral constraints for the current task. Before starting any non-trivial coding work:

1. **State what you're about to do** and any assumptions (Rule 1).
2. **Confirm the minimal scope** — no more, no less (Rule 2, Rule 3).
3. **Define your success criteria** before writing code (Rule 4).
4. **Read relevant code first** — callers, exports, utilities (Rule 8).
5. After each significant step, **checkpoint** (Rule 10).
6. **Fail loud**: if anything is skipped, uncertain, or incomplete, say so explicitly (Rule 12).

These rules do not compete with each other. When in doubt, pick the most cautious path and surface the tension.
