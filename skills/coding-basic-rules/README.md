# Coding Basic Rules — 12-Rule Engineering Discipline for Claude Code

A `SKILL.md` file enforcing 12 foundational engineering discipline rules on Claude Code behavior.

> **Source:** Adapted from [@Mnilax on X](https://x.com/Mnilax/article/2053116311132155938)

English | [简体中文](./README.zh.md)

---

## The Problem

LLMs coding assistants share a common set of failure modes:

> They assume rather than ask. They over-engineer when 50 lines would do. They touch code they shouldn't. They declare success when work is incomplete. They blend conflicting patterns instead of surfacing conflicts. They silently overrun token budgets and context limits.

These aren't model failures — they're behavioral defaults. Rules can override them.

---

## The Solution

12 principles in one file that directly address these failure modes:

| Rule | Addresses |
|------|-----------|
| **Think Before Coding** | Silent assumptions, hidden ambiguity, missing tradeoffs |
| **Simplicity First** | Overengineering, speculative features, premature abstraction |
| **Surgical Changes** | Orthogonal edits, unsolicited refactors, style drift |
| **Goal-Driven Execution** | Aimless step-following, unverifiable progress |
| **Model for Judgment Only** | Using LLM where deterministic code suffices |
| **Token Budgets Are Hard Limits** | Silent context overrun, loss of coherence |
| **Surface Conflicts, Don't Average** | Silent pattern blending, inconsistent codebases |
| **Read Before You Write** | Adding code without understanding callers/exports |
| **Tests Verify Intent** | Tests that pass even when business logic is broken |
| **Checkpoint Every Step** | Untracked state, losing the thread mid-task |
| **Match Codebase Conventions** | Taste-driven forks, inconsistent style |
| **Fail Loud** | Silent omissions, false "completed", skipped tests |

---

## The 12 Rules in Detail

### Rule 1 — Think Before Coding

**Don't assume. Don't hide confusion. Surface tradeoffs.**

- State assumptions explicitly — if uncertain, ask rather than guess
- Present multiple interpretations when ambiguity exists
- Push back when a simpler approach exists
- Stop when confused — name what's unclear before continuing

### Rule 2 — Simplicity First

**Minimum code that solves the problem. Nothing speculative.**

- No features beyond what was asked
- No abstractions for single-use code
- No "flexibility" that wasn't requested
- **The test:** Would a senior engineer call this overcomplicated? If yes, simplify.

### Rule 3 — Surgical Changes

**Touch only what you must. Clean up only your own mess.**

- Don't "improve" adjacent code, comments, or formatting
- Don't refactor what isn't broken
- Match existing style, even if you'd do it differently
- **The test:** Every changed line must trace directly to the user's request.

### Rule 4 — Goal-Driven Execution

**Define success criteria. Loop until verified.**

Instead of following steps, transform tasks into verifiable goals:

| Instead of... | Transform to... |
|--------------|-----------------|
| "Fix the bug" | "Write a test that reproduces it, then make it pass" |
| "Add validation" | "Write tests for invalid inputs, then make them pass" |
| "Refactor X" | "Ensure tests pass before and after" |

Strong success criteria enable autonomous looping. Weak criteria require constant clarification.

### Rule 5 — Use the Model Only for Judgment Calls

- Use for: classification, drafting, summarization, extraction
- Do NOT use for: routing, retries, deterministic transforms
- If code can answer, code answers

### Rule 6 — Token Budgets Are Not Advisory

- Per-task: 4,000 tokens. Per-session: 30,000 tokens.
- If approaching budget, summarize and start fresh
- **Surface the breach. Do not silently overrun.**

### Rule 7 — Surface Conflicts, Don't Average Them

- If two patterns contradict, pick one (more recent / more tested)
- Explain why. Flag the other for cleanup
- Don't blend conflicting patterns into a hybrid

### Rule 8 — Read Before You Write

- Before adding code, read exports, immediate callers, shared utilities
- "Looks orthogonal" is dangerous
- If unsure why code is structured a certain way, ask

### Rule 9 — Tests Verify Intent, Not Just Behavior

- Tests must encode **why** behavior matters, not just **what** it does
- A test that can't fail when business logic changes is wrong

### Rule 10 — Checkpoint After Every Significant Step

- Summarize what was done, what's verified, what's left
- Don't continue from a state you can't describe back
- If you lose track, stop and restate

### Rule 11 — Match the Codebase's Conventions, Even If You Disagree

- Conformance > taste inside the codebase
- If a convention is genuinely harmful, surface it — don't fork silently

### Rule 12 — Fail Loud

- "Completed" is wrong if anything was skipped silently
- "Tests pass" is wrong if any were skipped
- Default to surfacing uncertainty, not hiding it

---

## Install

**Option A: Claude Code Plugin Marketplace (recommended)**

Install directly from Claude Code in two commands:

```
/plugin marketplace add Adamatoma/cowork-skills
/plugin install coding-basic-rules@cowork-skills
```

Then invoke it by typing `/coding-basic-rules` in Claude Code.

**Option B: Manual Installation**

Clone and copy the skill to your Claude Code skills directory:

```bash
git clone https://github.com/Adamatoma/cowork-skills.git
cp -r cowork-skills/skills/coding-basic-rules/skills/coding-basic-rules ~/.claude/skills/coding-basic-rules
```

Then use it by typing `/coding-basic-rules` in Claude Code.

---

## Credits

Rules adapted from [@Mnilax](https://x.com/Mnilax/article/2053116311132155938) — a 12-rule engineering discipline template for LLM-assisted development.

Inspired by the broader discussion on LLM coding pitfalls, including [Andrej Karpathy's observations](https://x.com/karpathy/status/2015883857489522876) on model behavior defaults.
