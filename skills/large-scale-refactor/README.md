# Large-Scale Refactor — Multi-Session Memory Protocol for Claude Code

A `SKILL.md` that activates the **Structured Memory + Routed Context** framework, letting Claude Code carry architectural understanding across sessions on medium/large refactors and brownfield projects without rereading the whole repo every time.

> **Source:** Framework by 孟可丰 (2026), packaged here as a reusable skill.

English | [简体中文](./README.zh.md)

---

## The Problem

Long-running refactors with an LLM coding assistant fail in predictable ways:

> Every new session restarts from zero. Old and new patterns blend silently. Batches are declared "done" before docs catch up. The model loses the thread the moment the context window resets — and the next session inherits the confusion.

A bigger context window does not fix this. A small, durable **memory layer** the model rebuilds from at the start of every session does.

---

## The Solution

Two memory layers, one routing rule:

| Layer | File | Job |
|-------|------|-----|
| **Architecture** | Layered `readme.md` (root + each module) | What does this module own, what is its public surface, what must not break |
| **Continuation** | `continuation.md` | Where did we stop, what is verified, what's the next batch, what's queued |
| **(Audit log)** | `devlog.md` | Append-only batch summaries for human review |

Plus a routing rule: classify every potentially-relevant file as **必需 / 可选 / 不应给** before pulling it into the working set. The model loads the map first, then only the code the current task actually needs.

---

## How a session looks

```
Phase 0 — restore       continuation.md → root readme → module readme → state working set
Phase 4 — iterate       PLAN → READ → CODE → VERIFY → SYNC readme → ADVANCE continuation → LOG devlog → CHECKPOINT
Session end             update continuation + readme + devlog before /clear
```

The full phase map (0–5), working set protocol, anti-patterns, and quick reference live in [`SKILL.md`](./skills/large-scale-refactor/SKILL.md). Concrete templates for `readme.md`, `continuation.md`, and `devlog.md` live in [`references/templates.md`](./skills/large-scale-refactor/references/templates.md).

---

## Why it works

- **Read the map before the code.** Every session begins by restoring state from the memory layer, not by reopening the repo.
- **Touch code → touch readme.** The memory layer moves in lockstep with the code, in the same step. No silent drift.
- **Route, don't dump.** A working set classified as 必需 / 可选 / 不应给 keeps context focused and surfaces gaps loudly when something is missing.

Pairs with [`coding-basic-rules`](../coding-basic-rules/README.md): this skill governs *what to load and what to write down*; `coding-basic-rules` governs *how to actually edit code*.

---

## Install

**Option A: Claude Code Plugin Marketplace (recommended)**

```
/plugin marketplace add Adamatoma/cowork-skills
/plugin install large-scale-refactor@cowork-skills
```

Then invoke it by typing `/large-scale-refactor` in Claude Code.

**Option B: Manual installation**

```bash
git clone https://github.com/Adamatoma/cowork-skills.git
cp -r cowork-skills/skills/large-scale-refactor/skills/large-scale-refactor ~/.claude/skills/large-scale-refactor
```

Then use it by typing `/large-scale-refactor` in Claude Code.

---

## Usage cheat sheet

| Situation | Prompt |
|-----------|--------|
| Returning to an existing project | `/large-scale-refactor` + `续接上次任务，项目在当前目录` |
| New project, no memory layer yet | `/large-scale-refactor` + `新项目，帮我完成 Phase 1 建立记忆层` |
| Just finished a batch | `这批已完成，更新 readme 和 continuation` |
| Switching modules | `切换到 [模块名]，重新确认 working set` |
| Approaching context limit | `总结当前状态到 continuation，我要 /clear` |

---

## Credits

Framework adapted from 孟可丰 (2026) — *Structured Memory + Routed Context for AI-assisted refactoring*.

Inspired by the broader pattern of treating LLM context as a routed resource rather than a dumping ground.
