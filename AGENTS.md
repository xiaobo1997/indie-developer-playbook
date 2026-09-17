# AGENTS.md

> 本文件遵循 [AGENTS.md](https://agents.md) 开放约定。任何 AI 编码代理（Claude Code、Codex、Cursor、ZCode、Hermes 等）进入本仓库，先读本文件。

## 这个仓库是什么

一份中文的**独立开发者完整手册**：「一个人 = 一个团队」（PM、UI、前端、后端、运维、营销、客服）。它同时是作者本人的学习笔记仓库——作者正在从零开发微信小程序，实操经验持续沉淀回这里。

## 仓库结构（先看清分工，再动手）

```
phases/                 ★ 行动主线：8 个环节，从想法到收益，按顺序走
  README.md             # 环节地图 + 「我缺什么」自检清单
  01-validate/ … 08-iterate/
    README.md           # 环节说明（人读）
    references.md       # 外部参考链接（已验证）
    SKILL.md            # 可执行技能（AI 读并执行）
docs/                   知识库：按角色与平台组织的深度文档
  00~13-*/              # 总览/心态/产品/UI/前端/后端/DevOps/营销/客服/财务/平台/工作流/工具/案例
  decisions/            # 决策记录（ADR），重要约定的唯一事实来源
docs/10-platforms/      平台专项：wechat / wechat-game / ios / android / web-saas / self-hosted
skills/                 角色级技能库（部分建设中，状态见 skills/README.md）
examples/               真实独立开发者案例复盘（nomadlist / shipfast / plausible）
templates/              PRD、changelog 等模板
references/             人物、书籍、播客精选
CHANGELOG.md            变更日志（Keep a Changelog 格式）
```

**分工原则**：`phases/` 管时间线（什么时候做什么），`docs/` 管深度（某件事怎么做）。环节文档只链接 docs/，不复制内容。

## 给 AI 代理的核心指令

1. **执行环节任务**：当用户要求做某环节的事（验证需求/写 PRD/设计/选型/开发/上线/增长/复盘），先读对应的 `phases/XX-xxx/SKILL.md`，严格按其中的「输出契约」交付，执行前用「检查清单」自检。
2. **写内容时**：中文书写，代码与术语保留英文；平台规则、价格、限额等易变事实必须标注「以官方文档为准」并附官方链接。
3. **加外部链接时**：遵守 [ADR-0004](docs/decisions/0004-external-references-policy.md)——只加验证过的链接，优先官方与顶级开源仓库，每个链接附一句话说明。
4. **改结构、改约定时**：先看 `docs/decisions/`，与既有 ADR 冲突的方案不要自作主张提出；确需变更，起草一条新 ADR 交用户确认。
5. **完成实质修改后**：更新 `CHANGELOG.md` 的 `[Unreleased]` 小节（格式照抄现有条目）。
6. **发现文档间的矛盾或死链**：直接修复并在 CHANGELOG 记录。

## 写作与格式约定

- 语言：正文中文；目录名、代码、组件名用英文
- 环节文档固定小节：你要做什么 / 产出物 / 完成标准 / 常见陷阱 / 小程序主线注意点 / 深入学习
- SKILL.md 固定六段：任务目标 / 输入 / 输出 / 执行步骤 / 检查清单 / 边界；frontmatter 只写 `name` 与 `description`（见 [ADR-0002](docs/decisions/0002-agent-agnostic-skills.md)）
- 提交信息：`docs: ...` / `feat: ...` / `fix: ...` 前缀 + 中文摘要（沿用现有 git 历史）

## 新增一个环节（或子技能）的流程

1. 复制现有环节目录改三件套，或复制 `docs/decisions/0000-adr-template.md` 起草决策（若涉及结构变更）
2. 在 `phases/README.md` 地图、`CHANGELOG.md` 中登记
3. 链接必须互相打通：新页面要能从 README 两跳内到达
