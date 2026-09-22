# 11 · 工作流角色目录

> 工作流角色管"一个人怎么同时顶七个岗"：时间分配、角色切换纪律、AI 协作规则。它是横切所有角色的元角色。原则：**主题日、时间盒、80% 发布、一次一张卡**。
> 深度方法（角色日、番茄、决策框架、倦怠）见 [workflow.md](workflow.md)。

## 这个角色负责什么

| 负责 | 不负责 |
|---|---|
| 周节奏与角色日安排 | 替专业角色干活 |
| 时间盒与死线纪律 | 判断产品方向（PM 角色的数据说了算） |
| AI 协作规则（AGENTS.md / SKILL 流水线） | 工具选型（12-tools 的清单说了算） |

**边界判定**：产出物是"节奏与规则"；产出物是"交付物"就越界了。

## 独立跑完「工作流搭建」的流程

| 步 | 做什么 | 产出 | 耗时 |
|---|---|---|---|
| 1 | 定主题日：PM/开发/营销/复盘四类天 | 日历里重复事件 | 20 分钟 |
| 2 | 定时间盒：每天 ≤8 番茄（4 小时深度工作） | 作息写进日历 | 10 分钟 |
| 3 | 定周节奏：周一计划 / 周五复盘（30 分钟×2） | 两条重复事件 | 10 分钟 |
| 4 | AI 协作就位：项目放 AGENTS.md，环节任务喂 SKILL | AGENTS.md + SKILL 用法 | 半小时 |
| 5 | 红线挂墙上：连续 1 周零发布等四条（见心态角色） | 自检提醒 | 5 分钟 |

**AI 协作核心**（[ADR-0002](../decisions/0002-agent-agnostic-skills.md)）：每个环节/角色的 SKILL.md 是给 AI 的岗位说明书——对任何代理说「阅读 `phases/XX/SKILL.md` 并执行」即可；技能间靠输出契约衔接（上一环节输出 = 下一环节输入）。

## 收什么 → 交什么

| 上游交给我 | 我交给下游 |
|---|---|
| 各角色的进度与阻塞 | 每周日程（哪个角色哪天出场） |
| 环节 08 复盘的"下周只做 1-2 件事" | 时间盒分配（死线是工作流定的） |
| AGENTS.md 生态（[agents.md](https://agents.md) 约定、[Agent Skills 标准](https://agentskills.io/)） | 规则执行情况（自检数据） |

## 真实参考（不用凭空想象）

- 节奏范本：Marc Lou / Pieter Levels 的每日节奏对照见 [workflow.md](workflow.md) 的「推荐节奏」
- AI Skills 生态：[anthropics/skills](https://github.com/anthropics/skills)（官方技能仓库）、[obra/superpowers](https://github.com/obra/superpowers)（方法论框架，抄概念不引运行时）、[VoltAgent/awesome-agent-skills](https://github.com/VoltAgent/awesome-agent-skills)（按角色分类的技能清单）
- 多角色架构：本仓库选"静态技能树"而非运行时编排，见 [ADR-0005](../decisions/0005-roles-as-independent-skill-trees.md)；角色技能总览见 [skills/README.md](../../skills/README.md)
- 本仓库技能：工作流无独立 skill——它本身就是所有 SKILL 的调度层；周节奏执行走 [phases/08-iterate/SKILL.md](../../phases/08-iterate/SKILL.md)

## 新手最容易踩的坑

| 坑 | 为什么会踩 | 怎么避 |
|---|---|---|
| 过度计划 | 写 50 页 PRD 不写代码 | 时间盒：规划类任务 ≤半天 |
| 每天切 7 个角色 | 什么都想推进 | 主题日；上下文切换是隐性税 |
| 追新开多产品 | 新想法多巴胺 | 访谈数据提及 7 次；一次 1-2 个产品 |
| AI 产出不验收 | 生成很快有快感 | 验收三步是硬纪律（跑起来/看边界/读 diff） |
| 无死线的 side project | "做着做着就三年" | MVP 定 4-6 周死线，超了砍范围不延日期 |

## 本目录文件

| 文件 | 什么时候读 |
|---|---|
| [workflow.md](workflow.md) | 完整方法论：主题日、决策四问、工具栈、倦怠管理、推荐节奏 |
| [README.md](README.md)（本页） | 搭建自己的节奏与 AI 协作规则 |

## 带走清单（独立使用本目录时）

- `AGENTS.md`（仓库根）—— AI 协作约定的唯一事实源
- `docs/decisions/0002` + `0005` —— 技能格式与角色架构的来龙去脉
