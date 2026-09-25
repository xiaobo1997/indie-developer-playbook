# Skills - 可执行的 AI 工作流

> 每个 skill 是一个**指令集合**，可以让 AI 帮你完成具体工作。
> 技能分两类：**环节技能**（跟流程走，在 `phases/*/SKILL.md`）和**角色技能**（跟专业走，在本目录）。格式约定见 [ADR-0002](../docs/decisions/0002-agent-agnostic-skills.md)——纯 Markdown，兼容 Claude Code / Codex / Cursor / ZCode / Hermes 等任何 AI 代理。

## 角色 = 独立技能树

> 决策见 [ADR-0005](../docs/decisions/0005-roles-as-independent-skill-trees.md)（提议中）

**每个角色是一棵自包含的 Markdown 技能树，不与任何 Agent 运行时绑定。** 角色不是进程、不是子代理、也不是某个 harness 里的配置项——它就是 `skills/<角色>/` 下的一组文件。

三条硬规则：

1. **角色之间靠「输出契约」衔接，不靠调用关系**。A 角色的输出格式 = B 角色的输入格式（与 phases 环节之间的衔接机制一致）。谁来执行这份技能——人、Claude Code、Codex、Cursor、Hermes——**与本仓库无关**。
2. **技能文件里不出现任何 harness 特性**：slash command、子代理调度、专属变量、特定工具的 API 一律不用。
3. **分工清楚的判定标准**：能说清「A 交给 B 的是什么文件、什么格式」。说不清就是边界模糊——此时**先补契约，不要先补技能**。

代价要说清楚：拿不到并行执行的速度，每个技能都要人显式地喂给 Agent。换来的好处是——换任何 Agent、甚至不用 Agent，这套分工都成立；而且角色边界是可评审的文本，改起来改的是 Markdown 而不是代码。

## 状态图例

✅ 已完成 · ⏳ 建设中（README 已规划、文件待创建）

## 环节技能（★ 推荐从这里开始）

行动主线上的 8 个环节各有一个标准技能，见 [phases/](../phases/README.md)：

| 环节 | 技能 | 干什么 |
|---|---|---|
| 01 验证 | [validate-idea](../phases/01-validate/SKILL.md) ✅ | 竞品扫描 + 访谈提纲 + 做/不做结论 |
| 02 定义 | [write-spec](../phases/02-spec/SKILL.md) ✅ | 一页 PRD + ≤1 天任务卡 + 里程碑 |
| 03 设计 | [design-pages](../phases/03-design/SKILL.md) ✅ | 页面清单 + 组件映射 + 设计约定 |
| 04 脚手架 | [setup-project](../phases/04-setup/SKILL.md) ✅ | 选型决策表 + 项目骨架 + AGENTS.md |
| 05 开发 | [build-task](../phases/05-build/SKILL.md) ✅ | 单张任务卡实现 + 自测 + 验收步骤 |
| 06 上线 | [release-check](../phases/06-release/SKILL.md) ✅ | 发布前逐项审查 + 整改清单 |
| 07 增长 | [plan-launch](../phases/07-launch/SKILL.md) ✅ | 发布弹药包 + 4 周内容日历 |
| 08 迭代 | [weekly-review](../phases/08-iterate/SKILL.md) ✅ | 周复盘 + 迭代决策 + CHANGELOG |

## 多 Agent 协作技能 ⭐ 新增

> 一个人 = 一个团队。**角色 = 独立技能树**：没有主编进程、没有并行调度——需要哪个角色，就把哪个角色的技能文件喂给任何 Agent（或自己照着做）。架构约束见 [ADR-0005](../docs/decisions/0005-roles-as-independent-skill-trees.md)。
> 参照 obra/superpowers + VoltAgent + CrewAI 模式（只抄概念模型，不引运行时）。

| 角色 | 技能文件 | 职责 |
|------|----------|------|
| 任务拆分 | [multi-agent/orchestrator.md](multi-agent/orchestrator.md) ✅ | 把功能拆成按角色分工的任务卡 + 依赖 + 验收标准（**只产出任务卡，不调度、不派发**） |
| 前端 | [multi-agent/frontend.md](multi-agent/frontend.md) ✅ | UI / 组件 / 样式 |
| 后端 | [multi-agent/backend.md](multi-agent/backend.md) ✅ | API / 数据库 / 业务逻辑 |
| DevOps | [multi-agent/devops.md](multi-agent/devops.md) ✅ | Docker / K8s / CI/CD |
| 测试 | [multi-agent/tester.md](multi-agent/tester.md) ✅ | TDD / e2e / 性能 |
| 调试 | [multi-agent/debugger.md](multi-agent/debugger.md) ✅ | 系统 debug |
| 审查 | [multi-agent/reviewer.md](multi-agent/reviewer.md) ✅ | Code review |

完整 SOP 见 [multi-agent/SOP.md](multi-agent/SOP.md)。

## 🚢 一条龙发布（多平台）

- [ship/SKILL.md](ship/SKILL.md) ✅ - **主编排**：给代码 + 权限 → 预检 → 逐平台构建/部署/E2E/提审 → 带证据报告
- [ship/feasibility-check.md](ship/feasibility-check.md) ✅ - 可行性预检（强制第一关，先 go/no-go 再动手）
- 平台管线：[ship/platforms/](ship/platforms/)（web-h5 / miniprogram / minigame / ios / android / steam）+ [ship/ship-params.yaml](ship/ship-params.yaml)（参数与权限模板）

## 角色技能列表

### 🎯 产品 (Product)
- [product/user-interview](product/user-interview.md) ✅ - 用户访谈流程
- [product/prd-template](product/prd-template.md) ✅ - PRD 模板生成（结构对齐 [templates/prd-template.md](../templates/prd-template.md)）
- [product/user-story](product/user-story.md) ✅ - 用户故事编写（INVEST 自查）
- [product/evaluate-platforms](product/evaluate-platforms.md) ✅ - **做什么端评估**：端组合推荐 + 跨端路线 trade-off + 每端完整路径与总账（定案后交给 ship 执行）

### 🎨 UI
- [ui/design-review](ui/design-review.md) ✅ - 设计评审（六维只报问题，配 docs/03-ui/ai-as-designer.md）
- [ui/component-pattern](ui/component-pattern.md) ✅ - 组件映射（现成/组合/自写三类）

### 💻 前端 (Frontend)
- [frontend/tailwind-setup](frontend/tailwind-setup.md) ✅ - Tailwind 配置（仅 Web，小程序不适用）
- [frontend/responsive-checklist](frontend/responsive-checklist.md) ✅ - 多端适配检查

### ⚙️ 后端 (Backend)
- [backend/security-checklist](backend/security-checklist.md) ✅ - 安全清单
- [backend/api-design](backend/api-design.md) ✅ - API 契约设计（REST + 云开发等价契约）
- [backend/db-schema](backend/db-schema.md) ✅ - 数据库建模（含"明确不建的"）

### 🚀 DevOps
- [devops/deploy-checklist](devops/deploy-checklist.md) ✅ - 部署检查
- [devops/incident-response](devops/incident-response.md) ✅ - 事故响应 runbook
- [devops/backup-strategy](devops/backup-strategy.md) ✅ - 备份策略（3-2-1 + 恢复演练）

### 📣 营销 (Marketing)
- [marketing/launch-plan](marketing/launch-plan.md) ✅ - 渠道执行计划（策略定稿用 [phases/07-launch/SKILL.md](../phases/07-launch/SKILL.md)）
- [marketing/landing-page](marketing/landing-page.md) ✅ - 落地页文案与区块规划
- [marketing/content-calendar](marketing/content-calendar.md) ✅ - 发布后持续内容系统

### 🚀 发布 (Launch)
- [launch/product-hunt](launch/product-hunt.md) ✅ - Product Hunt 发布 runbook（仅 Web）
- [launch/twitter-launch](launch/twitter-launch.md) ✅ - 推特发布串（中英双版）
- [launch/press-kit](launch/press-kit.md) ✅ - 一页式媒体资料包

## 技能的参考来源（当年 ⏳ 时"先去抄"的出处，留作 provenance）

全部 19 个角色技能已补齐（3 个初始 + 16 个本轮新增）。以下保留各技能设计时参考的公开实现，感谢这些先行者：

| 技能 | 参考了 | 来源 |
|---|---|---|
| ui/design-review | `requesting-code-review` + `receiving-code-review`（「提」与「收」拆成两个 skill 的模式） | Superpowers |
| frontend/tailwind-setup | Frontend Design、UI engineering | VoltAgent awesome-agent-skills |
| frontend/responsive-checklist | UI engineering | VoltAgent |
| backend/api-design | API design | VoltAgent |
| backend/db-schema | database migration、data modeling | VoltAgent |
| devops/incident-response、backup-strategy | 本仓库 docs/06-devops 的监控与备份章节 | 本仓库 |
| marketing/* 、launch/* | [phases/07-launch/SKILL.md](../phases/07-launch/SKILL.md)（策略层）；角色技能专注执行层：launch-plan=渠道执行卡、content-calendar=发布后常态期 | 本仓库分层 |

来源仓库、star 数与编排模式见 [docs/11-workflow：角色分工](../docs/11-workflow/workflow.md#角色分工静态技能树不是运行时编排)。

### 一个结构性缺口：没有测试角色

现有 7 个角色（产品 / UI / 前端 / 后端 / DevOps / 营销 / 客服+财务）里**没有「测试」**。单人项目通常无所谓——你自己就是测试，`docs/04-frontend/frontend.md` 的「测试」一节也够用。

但一旦启用子代理并行开发，**测试角色是唯一能替你把关的人**：子代理会非常自信地交付没验证过的代码。Superpowers 用 `test-driven-development` 和 `verification-before-completion` 两个 skill 专门治这个。

是否要补 `skills/testing/` 目录，属于目录结构变更，需先起草 ADR（见 [docs/decisions](../docs/decisions/README.md)）。

## 新增 / 修订技能的规范

1. 结构固定六段：任务目标 / 输入 / 输出 / 执行步骤 / 检查清单 / 边界（抄 [phases/05-build/SKILL.md](../phases/05-build/SKILL.md) 的格式）
2. frontmatter 只写 `name` 与 `description`
3. 技能要薄，知识放 docs/：执行步骤里写明「先读 docs/XX」，渐进披露
4. 完成后更新本页对应条目为 ✅，并在 [CHANGELOG.md](../CHANGELOG.md) 记一笔
