---
name: large-scale-refactor
description: Large-scale project refactoring and secondary development protocol. Activates the Structured Memory + Routed Context framework so Claude Code can hand work off cleanly across sessions without losing architectural understanding. Use whenever the user starts or continues work on a medium/large codebase refactor, brownfield development, multi-module rewrite, or any task that will outlast a single conversation. Trigger on keywords like 重构, 二次开发, 大型项目, 跨会话, refactor, large codebase, brownfield, multi-session — and even when the user only describes the symptom ("代码改着改着就乱了" / "上次没接上") without naming a methodology.
---

# large-scale-refactor

Protocol for AI-assisted refactoring of medium/large codebases across multiple sessions.
**Core framework: Structured Memory + Routed Context (孟可丰, 2026).**

Always combine with `coding-basic-rules` during implementation — this skill governs *what to load and what to write down*; `coding-basic-rules` governs *how to actually edit code*.

---

## Why this skill exists

Long-running refactors fail in predictable ways: the model rereads the whole repo every session, blends old and new patterns, declares batches "done" before the docs catch up, and loses the thread the moment the context window resets. The fix is not a bigger context window — it is a small, durable memory layer the model rebuilds itself from at the start of every session, plus a discipline of only loading what the current task actually needs.

Three rules carry most of the weight:

1. **Read the map before the code.** Restore from `continuation.md` → root `readme.md` → relevant module `readme.md` *before* opening any source file.
2. **Touch code → touch readme.** The memory layer must move in lockstep with the code, in the same step.
3. **Classify every file** as Required / Optional / Exclude (必需 / 可选 / 不应给) before pulling it into the working set. When in doubt, leave it out and surface the gap.

---

## Decision tree — where do I start?

```
Is there a continuation.md or root readme.md describing the project?
├─ No  → Phase 1 (build the memory layer first; do not touch code yet)
├─ Partial (code exists, no map) → Phase 2 (extract structure into readme stubs)
└─ Yes → Phase 0 (restore state, then continue with Phase 4 batches)
```

If you are unsure which branch applies, run Phase 0 first — it is read-only and cheap; it will tell you whether the memory layer is sufficient.

---

## Phase 0 — Session Start Protocol

**Every session begins here. No exceptions.**

Before touching any code:

1. **Identify project phase** — which phase below are we in?
2. **Restore structure** — read in this order:
   - `continuation.md` → what was done, what's next
   - Root `readme.md` → architecture map
   - Relevant module `readme.md` files → current module state
3. **Confirm working set** — list the files you'll actually need this session, classified as Required / Optional / Exclude (必需 / 可选 / 不应给).
4. **State the session goal** — one verifiable sentence.

If any of these files are missing → switch to **Phase 1: Build Memory Layer** before doing anything else.

---

## Phase Map

| Phase | Name | Entry Condition | Exit Condition |
|-------|------|-----------------|----------------|
| 0 | Session Start | Beginning of every session | Memory restored, working set listed, goal stated |
| 1 | Build Memory Layer | No readme/continuation structure yet | Root + module readmes + continuation.md created |
| 2 | Legacy Structure Extraction | Brownfield: code exists, no map | All modules have readme stubs |
| 3 | Continuation Mechanism | Multi-session work begins | continuation.md tracks state reliably |
| 4 | Review-First Iteration Loop | Active refactoring | Each batch: code + readme + continuation updated |
| 5 | Compatibility Audit | Near completion | All contracts verified, no silent regressions |

---

## Memory Layer Architecture

Two layers, two jobs. Don't blur them.

### Layer 1 — Architecture Memory (layered readme)

**Location:** `readme.md` at root + each module directory.

Each module readme answers: *what does this module own, what is its public surface, what must not break?* The exact field set is in `references/templates.md`.

**Rules:**
- Each readme describes only its direct children — no cross-layer expansion.
- No history, no narrative, no emotional content.
- **Touch code → touch readme.** Always in the same commit/step.

### Layer 2 — Task Continuation Memory

**Location:** `continuation.md` (project root or `.ai/` directory).

`continuation.md` answers: *where did we stop, what is verified, what's the next batch, what's queued?* Full template in `references/templates.md`.

**Rules:**
- Never duplicate architecture facts (that's the readme's job).
- Never grow unbounded — prune completed items.
- Never become a second root readme.

### Source of Truth Priority

```
1. Current code          ← highest authority
2. Module readme
3. continuation.md
4. This session's prompts
5. Legacy code / old docs ← lowest, pull only when needed
```

When sources conflict: trust the higher layer, flag the conflict explicitly in `continuation.md`.

---

## Working Set Protocol

Before each task, classify all potentially relevant files:

| Class | Meaning | Include? |
|-------|---------|----------|
| **Required (必需)** | Task directly depends on it — omitting causes errors | Yes |
| **Optional (可选)** | Theoretically related, not needed this batch | Only if budget allows |
| **Exclude (不应给)** | Related domain but irrelevant to current task | No |

**Assemble working set in this order:**
1. Root rules / root readme
2. Most relevant module readme(s)
3. `continuation.md` (Current State section only)
4. Current code files (Required class only)
5. Legacy reference files (only when explicitly needed)

**Expand the working set only when:**
- Current files can't explain observed behavior.
- A readme and code contradict each other.
- A bug clearly crosses module boundaries.
- A compatibility gap appears and needs old code comparison.

---

## Iteration Loop (Phase 4)

Each batch follows this cycle:

```
1. PLAN       → state goal, list working set, define done criteria
2. READ       → read required files before writing anything
3. CODE       → implement (apply coding-basic-rules)
4. VERIFY     → run tests / build / manual check
5. SYNC       → update affected module readme(s)
6. ADVANCE    → update continuation.md (completed / next / risks)
7. LOG        → append entry to devlog.md (format in references/templates.md)
8. CHECKPOINT → summarize: what landed, what's verified, what's next
```

Steps 5, 6, and 7 are not optional. The memory layer and the log must stay in sync with the code, otherwise the next session restarts from a wrong picture.

`devlog.md` lives at the project root (or `.ai/devlog.md`). New entries are prepended (newest first). Past entries are never edited.

---

## Anti-Patterns (Fail Loud on These)

**AI anti-patterns — stop and surface if you catch yourself doing this:**
- Reading code before reading the readme map.
- Treating `continuation.md` as the source of truth for architecture.
- Defaulting to "reopen full repo" when working set is unclear.
- Blending old and new patterns without flagging the conflict.
- Declaring a batch "done" without updating readme + continuation.

**Human anti-patterns — flag these to the user:**
- Providing only a vague goal ("make it better", "refactor this").
- Changing code without updating the readme.
- Using one large doc to describe everything.
- Dumping the entire legacy codebase into context.

---

## Session End Protocol

Before ending any session:

1. Update `continuation.md` — current state, next batch, patch queue.
2. Update any readme files touched this session.
3. Append entry to `devlog.md` — batch summary in the template format.
4. State explicitly: what was completed, what was verified, what remains.
5. If anything was skipped or left uncertain — say so (Rule 12 of `coding-basic-rules`).

---

## Quick Reference

| Situation | Action |
|-----------|--------|
| New project, no memory layer | Phase 1: build readme + continuation.md before any code change |
| Brownfield, code exists but no map | Phase 2: extract structure into readme stubs |
| Starting a new session | Phase 0: restore from continuation → readme → code |
| Working set unclear | Classify files as Required / Optional / Exclude (必需 / 可选 / 不应给) before proceeding |
| readme and code conflict | Trust the code, update the readme, flag in continuation |
| Batch complete | Sync readme + continuation + devlog before the next batch |
| Context approaching limit | Summarize to continuation.md, /clear, resume |

---

## How to Use This Skill

### Starting a session (existing project)

```
/large-scale-refactor
Resume the previous task — project is in the current directory.
(中文：续接上次任务，项目在当前目录)
```

The model runs Phase 0: reads `continuation.md` → readme → restores state → reports current phase and next step before touching anything.

### Starting a session (new project, no memory layer yet)

```
/large-scale-refactor
New project, directory tree below — please complete Phase 1 and build the memory layer.
(中文：新项目，目录结构如下，帮我完成 Phase 1 建立记忆层)
```

The model scans the directory and generates readme skeletons + `continuation.md` *before* any refactoring begins.

### After completing a batch of code

```
This batch is done — sync readme and continuation.
(中文：这批已完成，更新 readme 和 continuation)
```

The model syncs the affected module readme(s), advances `continuation.md` (completed / next batch / patch queue), and appends a `devlog.md` entry.

### Before switching modules

```
Switching to [module name] — re-confirm the working set.
(中文：切换到 [模块名]，重新确认 working set)
```

The model re-classifies files as Required / Optional / Exclude for the new module before proceeding.

### When context is approaching the limit

```
Snapshot current state into continuation — I'm about to /clear.
(中文：总结当前状态到 continuation，我要 /clear)
```

The model writes a full state snapshot to `continuation.md`, then you run `/clear` and resume next session with `/large-scale-refactor` + a "resume previous task" prompt.

---

## Templates

Concrete skeletons for `readme.md`, `continuation.md`, and `devlog.md` live in `references/templates.md`. Read that file when bootstrapping the memory layer (Phase 1) or whenever you need to remind yourself of the exact field set.

---

**Rule: every session = activate skill first → restore state → then work. Never inspect code before the model has read the map.**
