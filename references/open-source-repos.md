# 开源仓库总索引

> 收录政策见 [ADR-0004](../docs/decisions/0004-external-references-policy.md)。
>
> **本页只做导航**。每个仓库的详细用法（什么场景去看、能抄什么）已按岗位下沉到 `docs/` 对应目录，本页不重复展开——这是 ADR-0001「链接不复制」的要求。改内容请去对应角色文档，本页只维护导航与 star 数。
>
> 所有仓库均通过 GitHub API 逐个验证（真实存在、未归档、有 License），star 数与更新时间截至 2026-09-18。

## 按岗位速查

| 你正在做的事 | 去哪看 |
|---|---|
| 搭 AI 协作流、写自己的 Skill | [11-workflow：AI 协作](../docs/11-workflow/workflow.md#ai-协作把-skills-当岗位说明书) |
| 让多个 AI 分角色并行干活 | [11-workflow：角色分工](../docs/11-workflow/workflow.md#角色分工静态技能树不是运行时编排) |
| 评估一个想法 / 做需求验证 | [02-product：开源参考](../docs/02-product/product-management.md#开源参考想法评估) |
| 选技术栈、压低 SaaS 月费 | [12-tools：开源替代](../docs/12-tools/stack.md#开源替代一人创业的工具栈) |
| 后端选型（数据库 / 鉴权 / 存储 / 邮件） | [05-backend：开源参考](../docs/05-backend/backend.md#开源参考) |
| 运维与省钱 | [06-devops：用开源替代](../docs/06-devops/devops.md#用开源替代具体到哪一项) |
| 找渠道、写发布内容 | [07-marketing：开源参考](../docs/07-marketing/marketing.md#开源参考) |
| 看别人的成败统计 | [13-cases：101 场创始人访谈](../docs/13-cases/case-studies.md#数据101-场创始人访谈) |
| 面对失败、要不要继续 | [01-mindset：这不是鸡汤，是统计](../docs/01-mindset/mindset.md#这不是鸡汤是统计) |

## 一、AI Agent Skills 生态

| 仓库 | star | 归属 |
|---|---|---|
| [anthropics/skills](https://github.com/anthropics/skills) | 176,940 | → [11-workflow：AI 协作](../docs/11-workflow/workflow.md#ai-协作把-skills-当岗位说明书) |
| [obra/superpowers](https://github.com/obra/superpowers) | 288,971 | → [11-workflow：角色分工](../docs/11-workflow/workflow.md#角色分工静态技能树不是运行时编排)（**15 个 skill，模式 A 的主力实现**） |
| [VoltAgent/awesome-agent-skills](https://github.com/VoltAgent/awesome-agent-skills) | 34,627 | → 同上（按角色分类的 skill 清单，补齐本仓库 ⏳ 技能的第一站） |
| [crewAIInc/crewAI](https://github.com/crewAIInc/crewAI) | 58,800 | → 同上（Python 库，抄它的「角色 + Crew 编排 + Task 序列」概念模型） |
| [smtg-ai/claude-squad](https://github.com/smtg-ai/claude-squad) | 8,500 | → 同上（多实例 TUI 管理器。⚠️ **AGPL-3.0**，商用前务必看清许可证） |
| [hesreallyhim/awesome-claude-code](https://github.com/hesreallyhim/awesome-claude-code) | 54,240 | → [11-workflow：AI 协作](../docs/11-workflow/workflow.md#ai-协作把-skills-当岗位说明书) |
| [travisvn/awesome-claude-skills](https://github.com/travisvn/awesome-claude-skills) | 15,102 | → 同上（⚠️ 最后更新 2026-04-28，部分外链可能腐坏） |

规范：[agents.md](https://agents.md)（AGENTS.md 开放约定，本仓库环节 04/05 已采用）、[Agent Skills 开放标准](https://agentskills.io/)（核心机制是渐进披露）。

> **star 数极易失真**：同一份资料里 Superpowers 被写成「89K+」，API 实测是 288,971（差 3 倍）。收录前一律 `curl https://api.github.com/repos/{owner}/{repo}` 核实。

## 二、独立开发 / 一人公司

| 仓库 | star | 归属 |
|---|---|---|
| [1c7/chinese-independent-developer](https://github.com/1c7/chinese-independent-developer) | 61,481 | → [13-cases：找同类的开源清单](../docs/13-cases/case-studies.md#找同类的开源清单) |
| [chen103226/awesome-one-person-company](https://github.com/chen103226/awesome-one-person-company) | 294 | → 同上 |
| [Micro-SaaS-Examples/Best-Micro-SaaS-Tools](https://github.com/Micro-SaaS-Examples/Best-Micro-SaaS-Tools) | 255 | → 同上 |
| [johackim/awesome-indiehackers](https://github.com/johackim/awesome-indiehackers) | 655 | → 同上 |
| [mezod/awesome-indie](https://github.com/mezod/awesome-indie) | 11,799 | → 同上（⚠️ 最后更新 2024-06-12，当路线图而非行动目录） |
| [yayashuxue/solo-founder-playbook](https://github.com/yayashuxue/solo-founder-playbook) | 25 | 按 skill 拆散：`solo-analyze` / `solo-roast` → [02-product](../docs/02-product/product-management.md#开源参考想法评估)；`solo-growth` → [07-marketing](../docs/07-marketing/marketing.md#开源参考)；`solo-failures` 的访谈数据 → [13-cases](../docs/13-cases/case-studies.md#数据101-场创始人访谈) |
| [rockscy/solo-skills](https://github.com/rockscy/solo-skills) | 8 | → [11-workflow](../docs/11-workflow/workflow.md#可以直接拿来用的单人技能)，其中 `launch-tweet` 归 [07-marketing](../docs/07-marketing/marketing.md#开源参考) |
| [sindresorhus/awesome](https://github.com/sindresorhus/awesome) | 507,256 | → [12-tools](../docs/12-tools/stack.md#开源替代一人创业的工具栈) |

## 三、全栈脚手架与工具栈

| 仓库 | star | 归属 |
|---|---|---|
| [wasp-lang/open-saas](https://github.com/wasp-lang/open-saas) | 15,860 | → [12-tools](../docs/12-tools/stack.md#开源替代一人创业的工具栈) |
| [princepal9120/awesome-solo-founder-oss](https://github.com/princepal9120/awesome-solo-founder-oss) | 64 | → 同上；其中后端四类另见 [05-backend](../docs/05-backend/backend.md#开源参考)、部署替代另见 [06-devops](../docs/06-devops/devops.md#用开源替代具体到哪一项) |

## 维护

- 每季度巡检一次本页链接；发现 404 直接删或替换（ADR-0004 第 4 条）
- 详细内容一律写在 `docs/` 对应角色目录，本页只加一行导航
- star 数会腐坏，「最后更新时间」更值得看——超过 12 个月没动的仓库，其收录的外链同样值得怀疑
