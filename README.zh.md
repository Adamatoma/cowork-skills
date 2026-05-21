# cowork-skills

> [English](README.md)

基于工作需要 & 实践，验证过的好用 Claude Code skill 合集。

## Installation

所有 skill 共享同一个插件市场入口，一次添加，按需安装：

```
/plugin marketplace add Adamatoma/cowork-skills
```

---

## Skills

### [gen-html-slides](./skills/gen-html-slides/README.md)

从零创建或将 PowerPoint 转换为动效丰富的 HTML 演示文稿。无需设计能力，通过可视化预览选风格，生成零依赖的单 HTML 文件。

![gen-html-slides demo](./demo/gen-html-slides/gen-html-slides-demo.gif)

![style selection](./demo/gen-html-slides/gif-style.gif)

**核心能力：**
- 零依赖 — 纯 HTML/CSS/JS，无需 npm 或构建工具
- 视觉风格探索 — 生成 3 个预览让你直接选，而非描述偏好
- PPT 转换 — 保留图片、文字、备注，一键转 Web
- 12 套精选主题 — 深色/浅色/特色，拒绝 AI 通用感

```
/plugin install gen-html-slides@cowork-skills
```

---

### [coding-basic-rules](./skills/coding-basic-rules/README.md)

12 条工程纪律规则，约束 Claude Code 在编码任务中的行为默认值。改编自经过实践验证的 LLM 编程规范。

![coding-basic-rules demo](./demo/coding-basic-rules/gif-coding-basic-rules.gif)

**核心能力：**
- 编码前思考，显式说明假设，不默默猜测
- 简洁优先，最少代码解决问题，不做投机性设计
- 精准修改，只动必须动的，不顺手"改进"无关代码
- 目标驱动执行，定义可验证的成功标准
- 大声失败，不隐藏不确定性，不虚报完成

```
/plugin install coding-basic-rules@cowork-skills
```

---

### [lark](./skills/lark/README.zh.md)

基于官方 lark-cli 工具，通过命令行操作飞书（Feishu）的全能技能。覆盖 7 大业务域：消息、文档、日历、任务、多维表格/Base、邮件、会议纪要。内置自我更新协议，每次使用后自动将验证过的 CLI 命令迭代进 skill，跨会话积累正确路径。

**核心能力：**
- 7 大业务域 — 消息、文档、日历、任务、多维表格/Base、邮件、会议纪要
- 一次性完整授权 — 安装时一次授权全部所需 scope，后续无需反复打开浏览器
- 自我校正 — 命令报错修正后自动写回 skill，跨会话积累正确路径
- 跨租户限制已记录 — 包含飞书个人版限制的绕过方案

```
/plugin install lark@cowork-skills
```

---

### [large-scale-refactor](./skills/large-scale-refactor/README.zh.md)

中大型重构与棕地项目的跨会话记忆协议。激活 **结构化记忆 + 路由上下文（Structured Memory + Routed Context）** 框架：用一个小而持久的记忆层（分层 readme + continuation + devlog）让模型在每次会话开始时重建状态，外加一条 working set 路由规则，每个文件先分类为 必需 / 可选 / 不应给 再加载。

**核心能力：**
- 两层记忆 — 架构层（readme）与续接层（continuation.md）职责严格分离
- Phase 0 会话启动协议 — 先从地图还原状态，再动任何代码
- 迭代循环强制同步 — 每个批次以更新 readme + continuation + devlog 结束，杜绝静默漂移
- 路由式 working set — 必需 / 可选 / 不应给 三类分类，让上下文保持聚焦，缺失时明确暴露
- 与 `coding-basic-rules` 配套使用 — 本 skill 管"加载与记录什么"，对方管"代码该怎么改"

```
/plugin install large-scale-refactor@cowork-skills
```

---

## 致谢

灵感来自「Vibe Coding」理念 —— 无需成为传统软件工程师，也能构建出美好的东西。

> 关注我的 X 账号：[@YutongPan3495](https://x.com/YutongPan3495)

## 许可证

MIT — 随意使用、修改、分享。
