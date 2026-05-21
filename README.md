# cowork-skills

> [中文](README.zh.md)

A curated collection of battle-tested Claude Code skills, built from real work needs.

## Installation

All skills share a single marketplace entry — add once, install on demand:

```
/plugin marketplace add Adamatoma/cowork-skills
```

---

## Skills

### [gen-html-slides](./skills/gen-html-slides/README.md)

Create stunning, animation-rich HTML presentations from scratch or by converting PowerPoint files. No design experience needed — pick your style from visual previews. Outputs a single, zero-dependency HTML file.

![gen-html-slides demo](./demo/gen-html-slides/gen-html-slides-demo.gif)

![style selection](./demo/gen-html-slides/gif-style.gif)

**Key Features:**
- Zero dependencies — pure HTML/CSS/JS, no npm or build tools required
- Visual style exploration — choose from 3 generated previews instead of describing preferences
- PPT conversion — preserves images, text, and speaker notes, converted to web in one step
- 12 curated themes — dark / light / featured, no generic AI look

```
/plugin install gen-html-slides@cowork-skills
```

---

### [coding-basic-rules](./skills/coding-basic-rules/README.md)

12 engineering discipline rules that constrain Claude Code's default behavior during coding tasks. Adapted from battle-tested LLM coding guidelines.

![coding-basic-rules demo](./demo/coding-basic-rules/gif-coding-basic-rules.gif)

**Key Features:**
- Think before coding — state assumptions explicitly, never guess silently
- Simplicity first — minimum code to solve the problem, no speculative design
- Surgical changes — only touch what must be touched, no opportunistic "improvements"
- Goal-driven execution — define verifiable success criteria
- Fail loudly — surface uncertainty, never fake completion

```
/plugin install coding-basic-rules@cowork-skills
```

---

### [lark](./skills/lark/README.md)

Operate Feishu (Lark) from the command line via the official lark-cli tool. Covers 7 business domains: messaging, documents, calendar, tasks, Base/multidimensional tables, mail, and meeting minutes. Includes a self-update protocol that automatically iterates verified CLI commands back into the skill across sessions.

**Key Features:**
- 7 domains covered — messaging, docs, calendar, tasks, Base/sheets, mail, meeting minutes
- One-time full authorization — authorize all scopes upfront, no repeated browser prompts
- Self-correcting — verified commands and discovered limitations are written back into the skill automatically
- Cross-tenant limitation documented — includes workarounds for Feishu Personal Edition restrictions

```
/plugin install lark@cowork-skills
```

---

### [large-scale-refactor](./skills/large-scale-refactor/README.md)

Multi-session memory protocol for medium/large refactors and brownfield development. Activates the **Structured Memory + Routed Context** framework: a small durable memory layer (layered readme + continuation + devlog) the model rebuilds from at every session start, plus a working-set routing rule that classifies every file as Required / Optional / Exclude (必需 / 可选 / 不应给) before loading.

**Key Features:**
- Two-layer memory — architecture (readme) and continuation (continuation.md), with strict role separation
- Phase 0 session-start protocol — restore state from the map *before* touching any code
- Iteration loop with mandatory sync — every batch ends by updating readme + continuation + devlog, no silent drift
- Routed working set — Required / Optional / Exclude classification keeps context focused and surfaces gaps loudly
- Pairs with `coding-basic-rules` — this skill governs what to load and record; the other governs how to edit code

```
/plugin install large-scale-refactor@cowork-skills
```

---

## Credits

Inspired by the "Vibe Coding" philosophy — building beautiful things without being a traditional software engineer.

> Follow me on X: [@YutongPan3495](https://x.com/YutongPan3495)

## License

MIT — Use it, modify it, share it.
