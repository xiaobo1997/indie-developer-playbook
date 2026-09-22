# 环节 02 · 外部参考

> 收录政策见 [ADR-0004](../../docs/decisions/0004-external-references-policy.md)。

## 范围控制与规格方法

- [Shape Up（Basecamp）](https://basecamp.com/shapeup) —— 免费在线书。「Appetite（先定时间预算再定范围）」「Fat Marker 草图」两章直接适用于本环节：倒过来做规划——只有 6 周，该砍什么。
- [templates/prd-template.md](../../templates/prd-template.md) —— 本仓库自带的一页 PRD 模板。
- [MakeBook（Pieter Levels《MAKE》）](https://makebook.io/) —— 付费小书，从想法到上线的完整单人流程，第 2 章讲"倒推式规划"。

## MVP 与砍功能的经典论述

- [The MVP is dead, long live the MVP](https://www.indiehackers.com/) —— 在 Indie Hackers 站内搜 "MVP"，多篇一手复盘讨论"最小可用"到底多小。
- [YAGNI（极端编程实践）](https://martinfowler.com/bliki/Yagni.html) —— Martin Fowler 对"You Aren't Gonna Need It"的经典阐述：为"将来可能要"写的代码是负债。
- [examples/shipfast 复盘](../../examples/shipfast/README.md) —— Marc Lou 的产品哲学：先发布，再完美；用 boilerplate 把"定义到开发"压缩到几天。

## 从 PRD 到 AI 任务卡

- [AGENTS.md](https://agents.md) —— 给编码代理写说明的开放约定。本环节产出的任务卡，格式上尽量对齐"AI 可执行"：一张卡 = 一次代理任务。
- [docs/decisions/0002](../../docs/decisions/0002-agent-agnostic-skills.md) —— 本仓库对"AI 可执行文档"的格式约定（六段结构、输出契约）。

## 微信小程序规划相关（官方）

- [小程序开发框架](https://developers.weixin.qq.com/miniprogram/dev/framework/) —— 页面层级、TabBar 数量、包体积等规划期约束的权威来源（以官方为准）。
- [小程序注册与主体说明](https://developers.weixin.qq.com/miniprogram/introduction/index.html) —— 个人/企业主体能力差异，PRD 阶段确认变现相关功能可行性。

## 工具生态（本环节能拿来用的）

> 列工具不是让你都用上，是让你知道**这个行业已经把哪些重复劳动做成了产品**。价格与可用性以官网为准（实测 2026-09-21 均可访问）。

| 工具 | 干什么 | 什么时候用 |
|---|---|---|
| [Notion](https://www.notion.so) | PRD + 知识库一体 | 写 PRD、存访谈记录，独立开发者免费额度够用 |
| [Linear](https://linear.app) | 任务卡（issue 跟踪） | 把 PRD 拆成任务卡；它的键盘流和默认字段很适合单人 |
| [Excalidraw](https://excalidraw.com) | 手绘风格流程图 / 草图 | 画页面关系、数据流；比 Word 快，比 Figma 轻 |
