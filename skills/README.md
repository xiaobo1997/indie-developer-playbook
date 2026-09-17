# Skills - 可执行的 AI 工作流

> 每个 skill 是一个**指令集合**，可以让 AI 帮你完成具体工作。
> 技能分两类：**环节技能**（跟流程走，在 `phases/*/SKILL.md`）和**角色技能**（跟专业走，在本目录）。格式约定见 [ADR-0002](../docs/decisions/0002-agent-agnostic-skills.md)——纯 Markdown，兼容 Claude Code / Codex / Cursor / ZCode / Hermes 等任何 AI 代理。

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

## 角色技能列表

### 🎯 产品 (Product)
- [product/user-interview](product/user-interview.md) ✅ - 用户访谈流程
- product/prd-template ⏳ - PRD 模板生成（先直接用 [templates/prd-template.md](../templates/prd-template.md)）
- product/user-story ⏳ - 用户故事编写

### 🎨 UI
- ui/design-review ⏳ - 设计评审
- ui/component-pattern ⏳ - 组件设计模式

### 💻 前端 (Frontend)
- frontend/tailwind-setup ⏳ - Tailwind 配置
- frontend/responsive-checklist ⏳ - 响应式检查

### ⚙️ 后端 (Backend)
- [backend/security-checklist](backend/security-checklist.md) ✅ - 安全清单
- backend/api-design ⏳ - RESTful API 设计
- backend/db-schema ⏳ - 数据库 schema 设计

### 🚀 DevOps
- [devops/deploy-checklist](devops/deploy-checklist.md) ✅ - 部署检查
- devops/incident-response ⏳ - 事故响应
- devops/backup-strategy ⏳ - 备份策略

### 📣 营销 (Marketing)
- marketing/launch-plan ⏳ - 发布计划（先直接用 [phases/07-launch/SKILL.md](../phases/07-launch/SKILL.md)）
- marketing/landing-page ⏳ - Landing Page
- marketing/content-calendar ⏳ - 内容日历

### 🚀 发布 (Launch)
- launch/product-hunt ⏳ - Product Hunt 发布
- launch/twitter-launch ⏳ - Twitter 公告
- launch/press-kit ⏳ - 媒体资料

## 补齐 ⏳ 技能的规范

1. 结构固定六段：任务目标 / 输入 / 输出 / 执行步骤 / 检查清单 / 边界（抄 [phases/05-build/SKILL.md](../phases/05-build/SKILL.md) 的格式）
2. frontmatter 只写 `name` 与 `description`
3. 完成后把本页对应 ⏳ 改成 ✅，并在 [CHANGELOG.md](../CHANGELOG.md) 记一笔
