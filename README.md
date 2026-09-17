# 独立开发者 Playbook

> **一个人 = 一个团队**
> 从零到独立开发者：产品、UI、前端、后端、运维、营销、客服

[![Made by One](https://img.shields.io/badge/Made%20by-One%20Person-blue)]() [![Status](https://img.shields.io/badge/Status-Active-success)]() [![Version](https://img.shields.io/badge/Version-0.1.0--alpha-orange)]()

## 这是什么？

一份**独立开发者完整工作手册**，包含：

- 🧭 **环节路线 (phases/)** - 从想法到收益的 8 个环节，每个环节 = 一个目录 = 说明 + 外部参考 + 可交给 AI 执行的 SKILL
- 📚 **文档 (docs/)** - 每个角色的深度知识库（含[决策记录 ADR](docs/decisions/)）
- 🛠 **技能 (skills/)** - 可复用的工作流和检查清单
- 📖 **参考 (references/)** - 精选人物、书籍、播客
- 💼 **案例 (examples/)** - 真实独立开发者的复盘
- 📋 **模板 (templates/)** - PRD、设计稿、合同等

> 📜 变更历史见 [CHANGELOG.md](CHANGELOG.md)；重要决定「为什么这么做」见 [docs/decisions/](docs/decisions/)。
> 🤖 AI 编码代理（Claude Code / Codex / Cursor / ZCode / Hermes 等）请先读 [AGENTS.md](AGENTS.md)。

## 适合谁？

- 程序员想自己做出完整产品
- 设计师想自己开发
- 产品经理想从想法到上线全程把控
- 任何想"一个人做出世界级产品"的人

## 阅读路径

### 🆕 新人（从零开始）
1. [00-总览](docs/00-overview.md) - 独立开发者是什么
2. 🧭 **[环节路线](phases/README.md)** - 从零到上线要做哪些事 + 「我缺什么」自检清单
3. 按环节走：[01 验证](phases/01-validate/) → [02 定义](phases/02-spec/) → [03 设计](phases/03-design/) → [04 脚手架](phases/04-setup/) → [05 开发](phases/05-build/) → [06 上线](phases/06-release/) → [07 增长](phases/07-launch/) → [08 迭代变现](phases/08-iterate/)
4. 卡在某个技能细节时，回来查对应角色的知识库（docs/）

### 🎯 有明确目标
按目录找你要的：
- 想做**小程序**？→ [10-platforms/wechat](docs/10-platforms/wechat/) + 各环节「小程序主线注意点」
- 想做 **iOS App**？→ [10-platforms/ios](docs/10-platforms/ios/)
- 想做**全栈 Web**？→ [10-platforms/web-saas](docs/10-platforms/web-saas/)
- 想**自建服务器**？→ [10-platforms/self-hosted](docs/10-platforms/self-hosted/)

### 🤖 想让 AI 帮你做？
- 读 [AGENTS.md](AGENTS.md) - 仓库级代理说明
- 对任意编码代理说：「阅读 phases/XX-xxx/SKILL.md 并执行」- 每个环节都有一个跨工具兼容的标准技能（见 [ADR-0002](docs/decisions/0002-agent-agnostic-skills.md)）

---

## 文档大纲

### 🧭 环节路线（行动主线）
| 环节 | 目录 | 一句话 |
|---|---|---|
| 01 需求验证 | [01-validate](phases/01-validate/) | 这个需求是真的吗？ |
| 02 产品定义 | [02-spec](phases/02-spec/) | 一页 PRD + 拆到 ≤1 天的任务卡 |
| 03 UI/UX 设计 | [03-design](phases/03-design/) | 组件库上做选择题，抄成熟产品 |
| 04 技术选型与脚手架 | [04-setup](phases/04-setup/) | 能跑、真机可预览的空项目 |
| 05 开发实现 | [05-build](phases/05-build/) | AI 写你验，每天可运行 |
| 06 部署与上线 | [06-release](phases/06-release/) | 合规过审 + 监控 + 可回滚 |
| 07 发布与增长 | [07-launch](phases/07-launch/) | 公开构建拿前 100 用户 |
| 08 数据迭代与变现 | [08-iterate](phases/08-iterate/) | 周复盘循环 + 点亮变现路线 |

### 🎯 基础（必读）
- [00-总览](docs/00-overview.md) - 独立开发者是什么
- [01-心态](docs/01-mindset/mindset.md) - 思维方式
- [11-工作流](docs/11-workflow/workflow.md) - 一个人怎么干活
- [12-工具栈](docs/12-tools/stack.md) - 推荐工具

### 🎨 7 大角色（核心技能）
| 角色 | 文档 | 技能 |
|---|---|---|
| **PM 产品** | [02-product](docs/02-product/) | [skills/product](skills/product/) |
| **UI/UX** | [03-ui](docs/03-ui/) | [skills/ui](skills/ui/) |
| **前端** | [04-frontend](docs/04-frontend/) | [skills/frontend](skills/frontend/) |
| **后端** | [05-backend](docs/05-backend/) | [skills/backend](skills/backend/) |
| **DevOps** | [06-devops](docs/06-devops/) | [skills/devops](skills/devops/) |
| **营销** | [07-marketing](docs/07-marketing/) | [skills/marketing](skills/marketing/) |
| **客服** | [08-support](docs/08-support/) | (建设中) |
| **财务** | [09-finance](docs/09-finance/) | (建设中) |

### 🌐 6 大平台（专项）
| 平台 | 文档 | 状态 |
|---|---|---|
| 微信小程序 | [wechat](docs/10-platforms/wechat/) | ✅ 完整 |
| 微信小游戏 | [wechat-game](docs/10-platforms/wechat-game/) | ⏳ 框架 |
| iOS App | [ios](docs/10-platforms/ios/) | ⏳ 框架 |
| Android App | [android](docs/10-platforms/android/) | ⏳ 框架 |
| Web SaaS | [web-saas](docs/10-platforms/web-saas/) | ⏳ 框架 |
| 自建服务器 | [self-hosted](docs/10-platforms/self-hosted/) | ⏳ 框架 |

### 🌟 真实案例
- [Nomad List](examples/nomadlist/) - Pieter Levels 的数字游民社区
- [ShipFast](examples/shipfast/) - Marc Lou 的启动套件
- [Plausible](examples/plausible/) - 隐私友好的分析工具

### 📜 治理
- [CHANGELOG.md](CHANGELOG.md) - 变更日志（什么变了）
- [docs/decisions/](docs/decisions/) - 决策记录 ADR（为什么这么做）
- [CONTRIBUTING.md](CONTRIBUTING.md) - 贡献指南
- [AGENTS.md](AGENTS.md) - AI 代理协作约定

---

## 快速开始

```bash
# 克隆仓库
git clone https://github.com/your-username/indie-developer-playbook.git
cd indie-developer-playbook

# 从环节路线开始（推荐入口）
open phases/README.md
```

---

## 如何贡献

我们欢迎贡献！请阅读 [CONTRIBUTING.md](CONTRIBUTING.md) 了解详情。

## 许可证

[MIT License](LICENSE) - 自由使用、修改、商业化。

---

## 致谢

本项目受以下启发：
- 🇳🇱 Pieter Levels (https://levels.io)
- 🇫🇷 Marc Lou (@marc_louvion)
- 🇺🇸 Indie Hackers (https://indiehackers.com)
