# 变更日志（Changelog）

本仓库遵循 [Keep a Changelog](https://keepachangelog.com/zh-CN/1.1.0/) 格式，版本号遵循语义化版本（SemVer）。

> **看什么变了，为什么变**：变更日志只记录「改了什么」；「为什么这么改」记录在 [docs/decisions/](docs/decisions/)（决策记录 ADR）。

## [Unreleased]

### 计划中
- 补齐 `skills/` 目录中 15 个尚未创建的角色技能文件（见 [skills/README.md](skills/README.md) 状态标注）

## [0.2.0] - 2026-09-18

### Added 新增
- **`phases/` 环节路线**：把「一个人做独立开发」拆成 8 个环节，每个环节一个目录，含三件套：
  - `README.md` 环节说明（做什么 / 产出 / 完成标准 / 陷阱）
  - `references.md` 外部参考（开源仓库、官方文档、成功案例链接）
  - `SKILL.md` 标准技能（纯 Markdown，兼容 Claude Code / Codex / Cursor / ZCode 等任意 AI 代理）
  - 环节：01 需求验证 → 02 产品定义 → 03 UI/UX 设计 → 04 技术选型与脚手架 → 05 开发实现 → 06 部署与上线 → 07 发布与增长 → 08 数据迭代与变现
- **`phases/README.md`**：环节地图总览 + 「从零开始自检：我缺什么」清单 + 小程序主线硬知识
- **`AGENTS.md`**：仓库根级 AI 代理说明（AGENTS.md 开放约定），任何编码代理进入仓库先读它
- **`docs/decisions/` 决策记录（ADR）**：0000 模板 + 0001~0004 四条初始决策
- **`CONTRIBUTING.md`**：贡献指南（修复 README 中失效的引用）
- 各环节 `references.md` 汇总了经验证的外部链接：开源 boilerplate、单人/双人成功产品的源码、微信小程序官方文档

### Changed 变更
- `README.md`：新增「环节地图」导航、变更日志与决策记录入口，快速开始指向 `phases/`
- `skills/README.md`：为技能文件增加 ✅/⏳ 状态标注，交叉链接到 `phases/*/SKILL.md`
- `docs/00-overview.md`：「立即开始」指向环节路线

### Decisions 决策（详见 docs/decisions/）
- [ADR-0001](docs/decisions/0001-phases-as-directories.md) 采用「环节 = 目录」的路线结构
- [ADR-0002](docs/decisions/0002-agent-agnostic-skills.md) SKILL 采用纯 Markdown + AGENTS.md 约定，最大化跨工具兼容
- [ADR-0003](docs/decisions/0003-miniprogram-first.md) 第一平台主线：微信小程序
- [ADR-0004](docs/decisions/0004-external-references-policy.md) 外部链接与事实性内容的收录政策

## [0.1.0] - 2026-09-17

### Added 新增
- 初始版本：docs/ 17 篇角色与平台文档（心态、7 大角色、6 大平台、工作流、工具栈、案例）
- examples/ 三个案例复盘（Nomad List、ShipFast、Plausible）
- templates/ PRD 与 Changelog 模板
- skills/ 首批 3 个技能（user-interview、deploy-checklist、security-checklist）
- references/ 精选人物、书籍、播客清单
