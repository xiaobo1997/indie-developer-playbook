# 11 - 一个人 = 一个团队 工作流

## 核心挑战

独立开发者面对的最大挑战不是任何单一技能，而是**同时维护多个角色**：
- 今天上午做 PM，下午写代码，晚上回客户邮件
- 上下文切换成本巨大

## 工作流设计

### 1. 角色日（Theme Days）

不要每天切换角色，按天分配：

```
周一：PM 日
  - 看数据、回复邮件
  - 规划本周任务
  - 用户访谈

周二/三：开发日
  - 编码（核心功能）
  - 深度工作

周四：营销 + 客服日
  - 发 Twitter、写博客
  - 回复客服消息
  - 公开构建日记

周五：复盘 + 探索日
  - 这周做了什么？
  - 数据如何？
  - 下周计划
  - 学习新技术
```

### 2. 时间块 (Time Blocking)

#### 早晨（黄金时间）
- 9:00-12:00：最重要的工作（通常是编码）

#### 下午
- 13:00-15:00：会议、沟通
- 15:00-17:00：次要工作（写文档、修复 bug）

#### 晚上
- 不工作（避免 burnout）

### 3. 番茄工作法变体

```
25 分钟深度工作
5 分钟休息
每 4 个番茄：15-30 分钟长休息
每天：最多 8 个番茄（4 小时深度工作）
```

---

## 项目管理

### 工具栈
- **任务管理**：Todoist / TickTick / Linear
- **文档**：Notion / Obsidian
- **时间追踪**：Toggl / Clockify
- **目标**：Yearly OKR

### 周节奏

#### 周一早晨（30 分钟）
- 回顾上周（做了什么、数据如何）
- 设定本周 3 个核心目标
- 列出本周要做的任务

#### 每天早上（10 分钟）
- 看今天的 3 件事
- 标记最重要的（MUST）

#### 每天晚上（10 分钟）
- 标记完成的任务
- 写下明天的 3 件事
- 简单写日记

#### 周五下午（30 分钟）
- 周回顾
- 数据分析
- 下周规划

---

## 决策框架

### 当不知道该不该做时

```
1. 这能 30 天内做完吗？
   否 → 砍功能
   是 → 继续

2. 有人愿意付钱吗？
   否 → 重新定位
   是 → 干

3. 我自己天天会用吗？
   否 → 想清楚再开始
   是 → MVP

4. 这能带来 100 个用户吗？
   否 → 不值得
   是 → 推进
```

### 砍功能清单（每个产品都砍）

在 MVP 阶段，问自己：
- 这个功能有人要吗？
- 没有它我会丢失用户吗？
- 这个功能能延后 6 个月吗？

如果不能回答"是"，**砍掉**。

---

## 工具栈整合

### 推荐的全套工具

```
PM:       Notion + Todoist
设计:     Figma + 即时设计
前端:     Cursor + v0.dev
后端:     Supabase + Cloudflare
DevOps:   Vercel + Sentry + Plausible
营销:     Buffer + ConvertKit
客服:     Crisp + HelpScout
财务:     Stripe + Wave
```

### 成本估算（月）
- 起步：$0-50
- 增长：$100-500
- 规模：$500+

---

## AI 协作：把 Skills 当岗位说明书

一个人要顶七个岗位，最现实的杠杆不是做得更快，而是把重复性的岗位动作固化成 **Skill**——一份给 AI 读的岗位说明书。本仓库 `phases/*/SKILL.md` 走的就是这条路，外部生态里已经有成体系的实践可以直接抄。

### 生态入口

| 仓库 | star | 什么场景去看 |
|---|---|---|
| [anthropics/skills](https://github.com/anthropics/skills) | 176,940 | **官方技能仓库**，起点站。含 docx/pdf/pptx/xlsx 文档处理、`skill-creator`（教你写技能的技能）、`webapp-testing`。注意文档类技能是 source-available 而非完全开源，商用前看清单个 skill 的许可证 |
| [obra/superpowers](https://github.com/obra/superpowers) | 288,242 | **agentic 开发方法论框架**（MIT）。工作流是：先逼出 spec → 拆成傻瓜都能执行的实现计划 → 子代理并行开发 + 逐项 review。强调真 TDD、YAGNI、DRY |
| [hesreallyhim/awesome-claude-code](https://github.com/hesreallyhim/awesome-claude-code) | 54,240 | 扩展生态总目录：skills / hooks / slash commands / agent orchestrators / MCP / plugins 六大类，找工具先翻它 |
| [travisvn/awesome-claude-skills](https://github.com/travisvn/awesome-claude-skills) | 15,102 | 另一份 skills 索引，偏桌面端工作流视角。⚠️ 最后更新 2026-04-28，部分外链可能腐坏 |

规范：[agents.md](https://agents.md)（本仓库环节 04/05 已采用的开放约定）、[Agent Skills 开放标准](https://agentskills.io/)（核心机制是渐进披露，直接解释了为什么本仓库 SKILL 的 frontmatter 只写两个字段）。

### 三个值得抄的设计模式

**1. Skill 与知识库分离。** 每个 skill 只是薄薄一层工作流，正文第一步恒定是「先读知识库」：

```
skills/solo-failures/
├── SKILL.md              # 只写工作流，模型每次都读
└── knowledge/
    ├── failure-modes.md  # 证据与细节，按需加载
    └── patterns.json     # 结构化数据，带精确计数
```

这正是本仓库 ADR-0002「渐进披露」的实践版。本仓库目前把角色知识放在 `docs/` 由 SKILL 链接，方向一致，但约定是隐含的——可以在 SKILL 的「执行步骤」里写明「先读 docs/XX」，让它落地而非靠默契。

**2. 反向触发 `Do NOT use when`。** 大多数 skill 只写「什么时候用」，[rockscy/solo-skills](https://github.com/rockscy/solo-skills)（★8，MIT，中英双语）额外写「什么时候别用」。原话：

> Force-applying a framework when it doesn't fit is the #1 way solo devs waste a Saturday.

本仓库 SKILL 的「边界」一节在做类似的事，但缺「什么输入进来时不要触发本技能」这一层。补上属于 SKILL 格式变更，需先起草 ADR。

**3. 每条结论挂证据。** 写「101 场访谈中提及 11 次」，不写「通常来说」。与 ADR-0004 第 3 条同源，只是用在了统计性断言上。

### 可以直接拿来用的单人技能

[rockscy/solo-skills](https://github.com/rockscy/solo-skills) 里几个专为「一个人发货」设计的：`ship-decision`（2-3 个方案快速拍板）、`changelog-from-commits`（git log 转用户视角发布说明）、`bug-from-user`（模糊用户反馈转可复现 bug）、`standup-solo`（5 分钟个人站会）。

### 角色分工：静态技能树，不是运行时编排

> **本仓库的立场（[ADR-0005](../decisions/0005-roles-as-independent-skill-trees.md)）：角色是一棵独立的 Markdown 技能树，不与任何 Agent 运行时绑定。** 下面 A、B 两种都是运行时编排，需要长期绑定某一个 harness（Claude Code / Codex / Hermes / Cursor）并按它的机制组织角色——**本仓库不采用**，列出来仅供对比。

「让多个 AI 分角色并行」听起来诱人，但它有个隐含前提：你得选定一个 harness 并接受它的调度机制（子代理、worktree 隔离、slash command）。换工具就整套失效——这正是 ADR-0002 放弃「工具专属格式」时用过的同一个理由，只是这次要解耦的对象从**文件格式**变成了**执行方式**。

本仓库的做法：**每个角色 = 一棵自包含的技能树**。角色之间不互相调用，靠**输出契约**衔接——A 角色的输出格式就是 B 角色的输入格式。谁来执行这份技能（人 / 任何 Agent）与本仓库无关。

| 模式 | 角色是什么 | 代价 |
|---|---|---|
| A. 主编 + 子代理 | 运行时进程，由主编调度并行 | 绑死 harness；review 压力推迟到最后一次爆发 |
| B. 角色 + 流程编排 | 框架里的对象，用代码串起来 | 引入 Python 库，仓库从手册变成代码项目 |
| **C. 角色 = 技能树（本仓库）** | **静态 Markdown 目录，靠输出契约衔接** | 没有并行速度；每个技能要显式喂给 Agent |

**为什么明知有代价还选 C**：单人项目的瓶颈从来不是「跑得不够快」，而是「验收不过来」——你只有一个大脑在做 review，同时开三个子代理只是把压力推迟到最后一次性爆发。分工清楚比跑得快重要得多。

判定分工是否清楚的标准只有一条：**任意两个角色之间，能不能说清「A 交给 B 的是什么文件、什么格式」。** 说不清就是边界模糊，此时先补契约，不要先补技能。

#### Superpowers：15 个 skill，不是 8 个

[obra/superpowers](https://github.com/obra/superpowers)（★288,971，MIT，最后更新 2026-09-19）是模式 A 最完整的实现。

**抄它的 skill 设计，不抄它的运行时调度**：下面这些技能本身是纯 Markdown，可以搬进本仓库的角色目录当素材；但它们依赖的「主编调度子代理」机制不要跟着搬（ADR-0005）。它的 `skills/` 下实为 **15 个**：

| 类别 | skill | 干什么 |
|---|---|---|
| 规划 | `brainstorming` | 动手前先把需求问清楚 |
| 规划 | `writing-plans` / `executing-plans` | 拆成傻瓜能执行的计划 / 照着执行 |
| 并行 | `dispatching-parallel-agents` | 调度多个并行 agent |
| 并行 | `subagent-driven-development` | 子代理驱动开发（**主力**） |
| 并行 | `using-git-worktrees` | 用 git worktree 隔开多个工作空间 |
| 质量 | `test-driven-development` | 真 TDD（先写测试再写实现） |
| 质量 | `verification-before-completion` | 完成前必须验证，不许自称完成 |
| 质量 | `requesting-code-review` / `receiving-code-review` | 请求评审 / 接收评审 |
| 排错 | `systematic-debugging` | 系统化调试，不许瞎猜 |
| 收尾 | `finishing-a-development-branch` | 收尾合并 |
| 元技能 | `using-superpowers` / `writing-skills` / `diagnosing-superpowers` | 用框架 / 写 skill / 诊断框架自身 |

两个容易被忽略但很关键：`using-git-worktrees`（**并行的前提**是把工作空间隔开，否则子代理互相覆盖改动）和 `verification-before-completion`（专治「AI 说做完了但其实没验证」——这是子代理模式下最常见的翻车方式）。

`requesting-code-review` 和 `receiving-code-review` 拆成两个 skill 是聪明的设计：**提评审和接评审是两种完全不同的心智模式**，塞进一个 skill 里两边都做不好。本仓库 `skills/` 里 ui/design-review 这类待补技能可以照这个思路拆。

#### 按角色去哪抄 skill

| 角色 | 抄什么 | 来源 |
|---|---|---|
| 前端 | Frontend Design、UI engineering、React Best Practices | [VoltAgent/awesome-agent-skills](https://github.com/VoltAgent/awesome-agent-skills)（★34,627，MIT） |
| 后端 | API design、database migration、TDD | 同上 + Superpowers |
| DevOps | Docker、K8s、CI/CD、infrastructure as code | 同上 |
| 测试 | TDD、e2e testing、performance testing | Superpowers + 同上 |
| 安全 | —— **本仓库已有**，直接用 [skills/backend/security-checklist](../../skills/backend/security-checklist.md) | 本仓库 |
| 数据 | ETL pipeline、data modeling | VoltAgent |

[CrewAI](https://github.com/crewAIInc/crewAI)（★58,800，MIT）本身是 Python 库，你大概率不会直接引它写代码，但它的**概念模型**（Agent 定义角色 + Crew 编排 + Task 序列）值得抄——尤其想把「规划 → 开发 → 验证」串成流水线时，参考它的分层。

#### 一个必须先看的东西：许可证

并行终端管理工具有些是 AGPL。[smtg-ai/claude-squad](https://github.com/smtg-ai/claude-squad)（★8,500）很好用——TUI 同时挂多个 Claude Code / Codex 实例、git worktree 隔离、一个面板看所有角色状态——但它是 **AGPL-3.0**：**如果你改了它用在自己的在线服务里，AGPL 要求你也开源相应的代码。**

对比：Superpowers、CrewAI、VoltAgent 全是 MIT。**做商业产品时先看许可证，再看 star 数。** 这也是 ADR-0004 把「有 License」列为收录硬指标的原因。

顺带一提：按 ADR-0005 与运行时解耦之后，这类工具的许可证风险其实也一并绕开了——本仓库不依赖任何 harness，自然不受它的许可证约束。上面这些内容只是「可以参考的素材」，不是「要装的东西」。

### 与倦怠相关的两个数据

101 场创始人访谈里，「过度劳累与孤立导致倦怠」被提及 **12 次**、「追新综合征」**7 次**。两者都落在工作流而非能力上，对策就在本页：[倦怠管理](#倦怠管理) 与 [砍功能清单](#砍功能清单每个产品都砍)。完整数据见 [13-cases](../13-cases/case-studies.md#数据101-场创始人访谈)。

---

## 多 Agent 协作（2026-09-20 新增）

一个人同时顶 7 个角色，上下文切换是最大成本。多 Agent 协作就是把每个角色固化成 **Skill**，让 AI 按需启动、独立工作、主编 Review。

### 核心架构

```
Hermes（主编）
  ├─ 拆任务 → 任务卡
  ├─ 派发 → 给对应角色 Agent
  ├─ Review → 收结果 + 审查
  └─ 合并 → 合入主分支

执行器（你操作的工具）：
  ├─ Cursor（编辑器）→ 前端/UI/代码细节
  ├─ Codex App（桌面）→ 全栈多 Agent 并行
  └─ Claude Code（编辑器）→ 后端/系统
```

### 触发词（在任何工具里说）

| 触发词 | 动作 |
|--------|------|
| `多角色开发 X` | Hermes 拆任务 → 派给 3 个角色 Agent 并行 |
| `单角色开发 X` | 只派一个角色，简单任务用 |
| `review 当前代码` | Hermes 启动 code-review 代理 |
| `debug 这个问题` | Hermes 启动 debug 代理 |
| `规划 X` | Hermes 启动 brainstorming 代理 |
| `切换到 X 角色` | 切换当前工作的角色上下文 |

### 角色 Agent 分配

| 角色 | 技能文件 | 来源 | 适用场景 |
|------|----------|------|----------|
| 主编 | `skills/multi-agent/orchestrator.md` | Superpowers + playbook | 拆任务、派发、Review |
| 前端 | `skills/multi-agent/frontend.md` | VoltAgent + Superpowers | UI / 组件 / 样式 |
| 后端 | `skills/multi-agent/backend.md` | Superpowers | API / 数据库 / 业务逻辑 |
| DevOps | `skills/multi-agent/devops.md` | awesome-agent-skills | Docker / K8s / CI/CD |
| 测试 | `skills/multi-agent/tester.md` | Superpowers | TDD / e2e / 性能 |
| 调试 | `skills/multi-agent/debugger.md` | Superpowers | 系统 debug |
| 审查 | `skills/multi-agent/reviewer.md` | Superpowers | Code review |

### 各工具怎么用

**Cursor**：装 skill 到 `.cursor/skills/`，用 Composer 模式 + 触发词。Cursor v2.4+ 支持 subagents 和 skills。

**Codex App**：装 skill 到 `~/.codex/skills/` 或仓库 `.codex/skills/`，打开仓库说触发词，Codex App 自动发现 skills，多 Agent 在独立 git worktree 并行。

**Claude Code**：装 skill 到 `.claude/skills/` 或用户级目录，触发词自动加载，subagent 天然支持并行。

**Hermes**：直接说触发词，Hermes 调 delegate_task 派给子代理。

### 工作流程

```
你说"多角色开发 X"
    ↓
Hermes 主编拆任务 → 生成任务卡
    ↓
派给 3 个 Agent 并行（前端/后端/DevOps）
    ↓
各自干活（独立工作空间）
    ↓
收结果 → 主编 Review
    ↓
通过 → 合入
失败 → debug 代理 → 重试
```

### 关键原则

1. **Hermes 只做规划和 Review，不写代码** — 你在编辑器里写
2. **Skills 到处跑** — SKILL.md 标准格式，三处通用
3. **任务卡原则** — 一张卡一次交付，验收通过再领下一张
4. **git worktree 隔离** — 每个角色在自己的分支上干活
5. **不越界** — 任务卡之外的改动要显式声明

---

## 节奏感

### 长期主义

```
月 1：想法 + 调研
月 2：MVP 设计
月 3-4：开发 + 测试
月 5：上线 + 第一批用户
月 6：迭代 + 优化
月 7-12：增长
```

### 警惕的节奏

#### 红线
- 连续 1 周没发布任何东西
- 3 个月没新用户
- 6 个月没收入
- 1 年还没盈利

#### 处理
- **重新评估**：是不是 PMF 没找到？
- **小步迭代**：而不是放弃

---

## 时间管理陷阱

### 常见坑
1. **过度计划**：写 50 页 PRD 不写代码
2. **完美主义**：反复改设计不发布
3. **无限制学习**：永远在"准备"
4. **功能蔓延**：一直加功能
5. **过早优化**：用户不到 100 就考虑分布式

### 对策
- **时间盒**：每个任务固定时间
- **80% 原则**：80% 完成就发布
- **动手 > 思考**：先做出来再优化

---

## 倦怠管理

### 早期信号
- 周日晚上焦虑
- 早上不想打开电脑
- 对所有功能都提不起兴趣
- 睡眠质量下降

### 应对
- **强制休息**：每周至少 1 天完全不看工作
- **运动**：每天 30 分钟
- **社交**：和其他独立开发者聊天
- **改变节奏**：换不同的角色

### 长期
- 把工作看作马拉松，不是冲刺
- 接受不是每天都有产出
- 关注长期趋势，不是每日数据

---

## 推荐节奏

### Marc Lou 的节奏
```
- 每天发 1-2 条 Twitter
- 每天写 1 篇博客
- 每天发布 1 个产品更新
- 每周发 1 个 Newsletter
- 每月发布 1 个新功能
```

### Pieter Levels 的节奏
```
- 早上做最重要的产品工作
- 下午做营销和沟通
- 每天写开发日记
- 每月启动 1 个新项目（大部分失败）
```

### 国内独立开发者
- 重视产品本身，不要沉迷营销
- 5% 时间做营销，95% 时间做产品
- 但要持续公开记录

---

## 推荐阅读

### 书
- 《Deep Work》- 深度工作
- 《Atomic Habits》- 习惯
- 《The One Thing》- 专注
- 《Essentialism》- 极简主义

### 必订阅
- Lenny's Newsletter
- IndieHackers
- Console by levelsio

