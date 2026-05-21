# Large-Scale Refactor — Claude Code 跨会话记忆协议

一个 `SKILL.md` 文件，激活 **结构化记忆 + 路由上下文（Structured Memory + Routed Context）** 框架，让 Claude Code 在中大型重构和棕地项目中跨会话保持架构理解，无需每次重新读取整个仓库。

> **来源：** 框架由孟可丰（2026）提出，本仓库将其打包为可复用 skill。

[English](./README.md) | 简体中文

---

## 问题所在

LLM 编程助手在长周期重构中有一组共通的失效模式：

> 每开新会话都从零开始。新旧模式被悄悄混合。文档还没跟上，批次就被宣告完成。一旦上下文窗口重置，模型就丢失了线索 —— 下一会话继承所有混乱。

更大的上下文窗口解决不了这个问题。能解决的是一个小而持久的 **记忆层**，让模型在每次会话开始时从中重建状态。

---

## 解决方案

两层记忆 + 一条路由规则：

| 层级 | 文件 | 职责 |
|------|------|------|
| **架构层** | 分层 `readme.md`（根目录 + 每个模块） | 这个模块拥有什么、公共接口是什么、什么不能破 |
| **续接层** | `continuation.md` | 上次停在哪、验证了什么、下一批做什么、什么排队中 |
| **（审计日志）** | `devlog.md` | 仅追加的批次摘要，供人类回看 |

外加一条路由规则：每个潜在相关的文件都要先分类为 **必需 / 可选 / 不应给**，再决定是否纳入 working set。模型先读地图，再按当前任务的实际需要拉代码。

---

## 一次会话长什么样

```
Phase 0 — 还原状态       continuation.md → 根 readme → 模块 readme → 列出 working set
Phase 4 — 迭代循环       PLAN → READ → CODE → VERIFY → SYNC readme → ADVANCE continuation → LOG devlog → CHECKPOINT
会话结束                 更新 continuation + readme + devlog，再 /clear
```

完整的 Phase Map（0–5）、Working Set 协议、反模式清单、速查表见 [`SKILL.md`](./skills/large-scale-refactor/SKILL.md)。`readme.md`、`continuation.md`、`devlog.md` 的模板骨架见 [`references/templates.md`](./skills/large-scale-refactor/references/templates.md)。

---

## 为什么有效

- **先读地图，再读代码。** 每次会话从记忆层还原状态开始，而不是重新打开整个仓库。
- **改代码 → 改 readme。** 记忆层与代码同步移动，在同一步完成，杜绝静默漂移。
- **路由，而非倾倒。** working set 被严格分类为 必需 / 可选 / 不应给，让上下文保持聚焦，缺失时明确暴露。

与 [`coding-basic-rules`](../coding-basic-rules/README.zh.md) 配套使用：本 skill 决定 *加载什么、记录什么*；`coding-basic-rules` 决定 *代码该怎么改*。

---

## 安装

**方式 A：Claude Code 插件市场（推荐）**

```
/plugin marketplace add Adamatoma/cowork-skills
/plugin install large-scale-refactor@cowork-skills
```

之后在 Claude Code 中输入 `/large-scale-refactor` 即可调用。

**方式 B：手动安装**

```bash
git clone https://github.com/Adamatoma/cowork-skills.git
cp -r cowork-skills/skills/large-scale-refactor/skills/large-scale-refactor ~/.claude/skills/large-scale-refactor
```

之后在 Claude Code 中输入 `/large-scale-refactor` 即可调用。

---

## 用法速查

| 场景 | 提示词 |
|------|-------|
| 续接已有项目 | `/large-scale-refactor` + `续接上次任务，项目在当前目录` |
| 新项目尚无记忆层 | `/large-scale-refactor` + `新项目，帮我完成 Phase 1 建立记忆层` |
| 刚完成一个批次 | `这批已完成，更新 readme 和 continuation` |
| 切换到其他模块 | `切换到 [模块名]，重新确认 working set` |
| 上下文接近上限 | `总结当前状态到 continuation，我要 /clear` |

---

## 致谢

框架改编自孟可丰（2026）—— *Structured Memory + Routed Context for AI-assisted refactoring*。

灵感来自把 LLM 上下文当作"被路由的资源"而非"倾倒场"的整体范式。
