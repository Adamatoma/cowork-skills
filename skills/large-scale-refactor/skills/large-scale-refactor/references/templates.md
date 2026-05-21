# Templates — readme / continuation / devlog

Concrete skeletons for the memory layer used by `large-scale-refactor`. Copy and edit these when bootstrapping a project (Phase 1) or when you need to remind yourself of the exact field set.

---

## Module `readme.md`

Each module directory (and the project root) gets a `readme.md` with these fields. Keep it factual and short — this is a map, not a tutorial.

```markdown
# <module name>

role:        <what this module does, one sentence>
status:      draft | partial | landed | stable
files:       <current file list, or a glob>
surface:     <public API / exported symbols>
contracts:   <key invariants that must not break>
boundaries:  <what belongs here / what does not>
use:         <recommended usage patterns>
avoid:       <anti-patterns, known traps>
deps:        <internal + external dependencies>
traps:       <common mistakes, sharp edges>
```

**Rules:**
- Each readme describes only its direct children. Do not pull in grandchildren.
- No history, no narrative, no "we tried X and it didn't work" — that goes in `devlog.md`.
- When the code changes, update the readme in the same step.

---

## `continuation.md`

Lives at the project root or under `.ai/`. This is the file the next session reads first.

```markdown
## Current State
- Last completed: <module/task>
- Verified working: <what was tested, how>
- In-progress: <what was started but not finished>

## Next Batch
- Recommended next: <specific module or task>
- Files to request: <list>
- Known risks: <list>

## Patch Queue
- <item>: needs follow-up because <reason>
- <item>: needs follow-up because <reason>

## Open Conflicts (optional)
- <readme vs code / old vs new pattern>: <how to resolve>
```

**Rules:**
- Never duplicate architecture facts (readme owns those).
- Prune completed Patch Queue items — do not let this file grow without bound.
- If it starts looking like a second root readme, delete the duplicated parts.

---

## `devlog.md`

Append-only log of batches. New entries are prepended (newest first). Past entries are never edited.

```markdown
## [YYYY-MM-DD] batch | <one-line goal>

- Completed: <what was implemented/changed>
- Files: <list of modified files>
- Verified: <how it was tested>
- Next: <what continuation.md says is next>
```

**Rules:**
- One entry per batch, written at step 7 of the iteration loop.
- Keep entries short — the goal is "what happened", not "why we chose X". Reasoning lives in commit messages or design docs.

---

## Worked example — first time setup (Phase 1)

For a fresh project, the minimum viable memory layer is:

```
project-root/
├── readme.md            ← root architecture map
├── continuation.md      ← starts with Current State = "Phase 1 setup", Next Batch = first module
├── devlog.md            ← starts empty (or with a single "Phase 1 setup" entry)
└── <module-a>/
    └── readme.md        ← role/status/files/surface/...
```

Build these *before* writing or refactoring any code. Phase 0 of the next session will rely on them.
