# docs/ · 角色知识库

> 本目录按**角色**组织，与 `phases/` 的**环节**主线正交。
> 分工原则（[ADR-0001](decisions/0001-phases-as-directories.md)）：`phases/` 管时间线（什么时候做什么），`docs/` 管深度（某件事怎么做）。两边互相链接，不复制内容。

## 怎么挑：按你要做的事

| 我要…… | 去哪 |
|---|---|
| 判断一个需求真假、写 PRD | [02-product](02-product/product-management.md) |
| 调整心态、面对失败想放弃 | [01-mindset](01-mindset/mindset.md) |
| **做界面、不知道 UI 该长什么样** | [03-ui](03-ui/README.md) ← 第一个按自包含规范建好的样板 |
| 写前端页面、选型 | [04-frontend](04-frontend/frontend.md) |
| 做后端、数据库、鉴权 | [05-backend](05-backend/backend.md) |
| 部署、监控、省钱 | [06-devops](06-devops/devops.md) |
| 找渠道、写发布内容 | [07-marketing](07-marketing/marketing.md) |
| 处理用户反馈 | [08-support](08-support/support.md) |
| 定价、收款、算现金流 | [09-finance](09-finance/finance.md) |
| 选某个平台（小程序 / iOS / SaaS） | [10-platforms](10-platforms/) |
| 排工作流、防倦怠、用 AI 协作 | [11-workflow](11-workflow/workflow.md) |
| 挑工具栈 | [12-tools](12-tools/stack.md) |
| 看别人的成败 | [13-cases](13-cases/case-studies.md) |
| 查「为什么这么定」 | [decisions/](decisions/README.md) |

## 角色目录的自包含结构

**每个角色目录都应该是「能独立拿走用」的**——有流程、有真实参考、有边界，读完就知道这个角色该干什么、怎么干，而不是一堆互相链接的半成品。

规范结构（完整实现见 [03-ui/README.md](03-ui/README.md)：

1. **职责边界**——负责什么 / 不负责什么，以及边界怎么判
2. **独立跑完对应环节的流程**——分步骤，每步写清产出与耗时
3. **收什么 → 交什么**——与上下游角色的输出契约（这是角色之间唯一的衔接方式，见 [ADR-0005](decisions/0005-roles-as-independent-skill-trees.md)）
4. **真实参考**——开源项目与官方文档，**让人不用凭空想象**
5. **新手最容易踩的坑**——表格：坑 / 为什么会踩 / 怎么避
6. **本目录文件索引**
7. **带走清单**——单独拷走这个目录时，还需一并带哪些文件

第 4 条是重点：**不要让人凭空想象**。给得出链接就给链接，给不出就明说「暂无，先参考 XX」。

## 目录与补齐状态

| 目录 | 角色 | 自包含 README |
|---|---|---|
| [00-overview](00-overview.md) | 总览 | —（单文件，不需要） |
| [01-mindset](01-mindset/README.md) | 心态 | ✅ |
| [02-product](02-product/README.md) | PM | ✅ |
| [03-ui](03-ui/README.md) | UI/UX | ✅ **（样板，含 3 篇深度文档）** |
| [04-frontend](04-frontend/README.md) | 前端 | ✅ |
| [05-backend](05-backend/README.md) | 后端 | ✅ |
| [06-devops](06-devops/README.md) | DevOps | ✅ |
| [07-marketing](07-marketing/README.md) | 营销 | ✅ |
| [08-support](08-support/README.md) | 客服 | ✅ |
| [09-finance](09-finance/README.md) | 财务 | ✅ |
| [10-platforms](10-platforms/README.md) | 平台专项 | ✅（索引页；6 个平台目录各有完整指南） |
| [11-workflow](11-workflow/README.md) | 工作流 | ✅ |
| [12-tools](12-tools/README.md) | 工具栈 | ✅ |
| [13-cases](13-cases/README.md) | 案例 | ✅ |

深化方向：各 README 目前是「流程 + 契约 + 参考 + 坑」的自包含入口，**深度文档仍以单篇为主**。想继续加强某个角色，学 [03-ui](03-ui/README.md) 的做法——往目录里加深度文档（如 ai-as-designer.md），而不是把 README 写长。
