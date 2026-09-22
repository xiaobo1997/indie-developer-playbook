# 变更日志（Changelog）

本仓库遵循 [Keep a Changelog](https://keepachangelog.com/zh-CN/1.1.0/) 格式，版本号遵循语义化版本（SemVer）。

> **看什么变了，为什么变**：变更日志只记录「改了什么」；「为什么这么改」记录在 [docs/decisions/](docs/decisions/)（决策记录 ADR）。

## [Unreleased]

### Added 新增
- **`skills/multi-agent/`**：多 Agent 协作 SOP + 7 个角色技能（主编/前端/后端/DevOps/测试/调试/审查）
  - 参照 obra/superpowers + VoltAgent/awesome-agent-skills + CrewAI 模式
  - SKILL.md 格式，兼容 Hermes / Codex App / Cursor / Claude Code
  - 触发词：`多角色开发 X` / `单角色开发 X` / `review 当前代码` / `debug 这个问题`
  - 全部链接经 GitHub API 逐个验证（存在性 / 未归档 / 有 License），star 数与更新时间截至 2026-09-18
  - 记录从 `solo-founder-playbook`、`rockscy/solo-skills` 学到的三个 Skill 设计模式：Skill 与知识库分离（渐进披露）、反向触发 Do NOT use when、每条结论挂证据
  - 把 101 场创始人访谈的「7 大失败模式 + 7 个反模式」逐条映射到本仓库的环节与角色文档
- `phases/01-validate/references.md` 与 `phases/08-iterate/references.md` 分别新增「为什么要验证」「失败发生在哪一环」数据支撑小节
- `references/README.md` 顶部新增开源资料入口
- **多角色子代理编排资料**（已逐个用 GitHub API 核实，含纠正两处引用失真的常用数据）
  - `obra/superpowers` ★288,971（MIT）——实为 **15 个 skill**（常被写做 8 个），其中 `using-git-worktrees` 是并行隔离的前提、`verification-before-completion` 专治「AI 自称完成」；`requesting-` 与 `receiving-code-review` 拆成两个 skill 的设计值得抄
  - `VoltAgent/awesome-agent-skills` ★34,627（MIT）——按角色分类的 skill 清单，是补齐本仓库 15 个 ⏳ 技能的第一站
  - `crewAIInc/crewAI` ★58,800（MIT）——不引库，抄它的「Agent 角色 + Crew 编排 + Task 序列」概念模型
  - `smtg-ai/claude-squad` ★8,500——多实例 TUI 管理器，⚠️ **AGPL-3.0**，商用前必须看清许可证
  - 三种编排模式对比写进 `docs/11-workflow`（后按 ADR-0005 修订为：角色是静态技能树，不用运行时编排）
- **`docs/03-ui/ui-reference-projects.md`**：UI 角色的参考项目库（新增）
  - 13 个开源项目经 GitHub API 逐个核实，按「极简 Dashboard / 学习类 / SaaS 精品 / 微信小程序」四组组织，每组标注 star、许可证、学什么
  - 新增「独立应用的 6 种页面骨架」——回答「不知道独立应用 UI 到底长什么样」这个根本问题，每种都标注新手最容易做错什么
  - **纠正 3 处链接问题**：`calcom/cal.com` 已改名 `calcom/cal.diy`；`wechat-miniprogram/colorui-beta` 是 404（真实为 `weilanwl/coloruicss`，且已停更两年）；5 个项目实为 AGPL / GPL
  - 附许可证速查：看布局不受任何约束，复制代码进商业产品则需区分 MIT / AGPL-3.0 / GPL-3.0 / 自定义
  - `prototype-first.md` 的「抄成熟产品」改为优先推荐本仓库自建的参考库——**网站会打不开，仓库不会**

### Changed 变更
- `skills/README.md` 新增「补齐 ⏳ 技能时，先去抄」映射表，把每个待补技能指向现成的外部实现；并指出**现有 7 个角色缺「测试」**这一结构性缺口
- **开源资料按岗位下沉**：`references/open-source-repos.md` 改为纯导航索引（只留导航 + star 数），详细内容按角色拆进 `docs/` 各目录，遵守 ADR-0001「链接不复制」
  - `docs/13-cases`：101 场创始人访谈总表（7 大失败模式 + 7 个反模式，每条标注提及次数与防守位）+ 找同类的开源清单
  - `docs/11-workflow`：AI Skills 生态入口、三个值得抄的 Skill 设计模式、可直接用的单人技能、倦怠（12 次）与追新（7 次）数据
  - `docs/02-product`：「没做用户验证就开建」11 次提及 + 早期信号与不可挽回点 + `solo-analyze` / `solo-roast`
  - `docs/09-finance`：13 周现金预测（现金流断裂 13 次提及，是唯一会直接终止项目的模式）
  - `docs/12-tools`：开源替代一人创业的工具栈（open-saas / awesome-solo-founder-oss）
  - `docs/07-marketing`：分发策略反模式 + `solo-growth` / `launch-tweet`
  - `docs/01-mindset`：失败是统计不是鸡汤（15 次 / 本职工作安全网 8 次）
  - `docs/05-backend`、`docs/06-devops`：各自链到工具栈清单的对应分类
- **`templates/wireframe-preview.html`**：线框原型预览工具（单文件、打开即用）
  - 左侧写页面清单标记语法，右侧按 375×690 手机比例实时渲染线框
  - 支持多页面切换、空 / 加载 / 错误三态切换、导出 `pages.md`
  - 解决「构思阶段看不见页面」的问题：从想法到看得见只需几分钟
- **`docs/03-ui/prototype-first.md`**：原型优先方法论（新增）
  - 核心功能守门：一句话定义只能有一个动词；三个问题判功能该不该进 MVP
  - 扩展性边界：只有下个月就要做的才值得留接口（附该预留 / 不该预留对照表）
  - 反花哨硬标准：去掉它任务还能完成就是装饰；灰度化测试验证信息层级
  - 一个人怎么「找」UI / UED / UX：抄成熟产品、外包给组件库、AI 生成、技能交换、5 人测试替代 UX 专家
- `docs/03-ui/ui-design.md` 顶部、`phases/03-design/README.md` 第 2 步改为「可预览线框」并接入新工具
- **角色目录改为自包含结构**（「能独立拿走用」，而不是一堆互相链接的半成品）
  - 新增 `docs/03-ui/README.md`（111 行）——UI 角色入口，第一个按规范建好的样板
  - 新增 `docs/README.md`（60 行）——按「我要做什么」挑角色的索引 + 自包含七要素规范 + 各目录补齐状态
  - `phases/03-design/README.md` 新增「真实参考（不用凭空想象）」区块，直接给出 4 个可点开的开源仓库 + 6 种页面骨架提示
  - **核心原则写进规范：不要让人凭空想象。给得出链接就给链接，给不出就明说「暂无，先参考 XX」**
- **`docs/03-ui/ai-as-designer.md`**：让 AI 当设计师的完整流程（新增）
  - 核心原则：**不要把「设计」当独立阶段**，它是需求 → 用户流程 → 低保真 → Design System → 高保真 → 代码 → 真实截图 → 继续调整的连续收敛
  - 四层分工：原型解决「怎么用」/ UI 解决「长什么样」/ Design System 解决「怎么保持一致」/ Icon Library 解决「怎么快速完成细节」
  - **分层 prompt 模板**：信息架构 → 页面列表 → 布局 → 组件 → tokens → UI，每步确认再往下；附反例「帮我设计一个漂亮的网站」会得到 Dribbble 页面
  - **补上此前漏掉的闭环**：代码写完把真实截图回给 AI 评审（附评审 prompt，含「不要泛泛夸」）
  - 划清 Figma 的两种用法：视觉确认 ✅ / 像素精修 ❌——修正了「Figma 是负资产」这句说过头的话
- `docs/03-ui/ui-reference-projects.md` 新增 **E 组「AI 生成 UI 的工具」**：12 个工具官网逐个实测 HTTP 状态码后收录
  - **明确标注这些工具全都不支持微信小程序**（产出 Web 组件，小程序是 WXML / WXSS），做小程序直接看 D 组
  - 实测发现 **Galileo AI 官网已 000（连不上）**：虽被标注「已被 Canva 收购」，独立产品已不可访问，不该再进工具链——正好印证「工具会被收购关停，仓库地址不会」
  - 区分 403（站点反爬，仍可用）与 000（真的连不上），避免误判
  - 不收图像生成模型的具体版本号（半年即过时），只说明产出是「概念图不是可开发的东西」= [ai-as-designer.md](docs/03-ui/ai-as-designer.md) 说的 Dribbble 页面
- **8 个环节的 `references.md` 各新增「工具生态」小节**（01 验证 / 02 定义 / 03 设计 / 04 脚手架 / 05 开发 / 06 上线 / 07 增长 / 08 迭代）
  - 统一三列：**工具 / 干什么 / 什么时候用**；收录前逐个实测 HTTP 可访问性（22 个里 21 个可用，TDesign 只有 `www` 子域不通，正式域名正常）
  - 立下「**环节主线 × 工具生态**」两条线的说法并写进 `phases/README.md`：环节管「什么时候做什么」（慢变量，三年后仍成立），工具管「现在有什么能拿来用」（快变量，会被收购关停改价）——**两者不互相替代**
  - 03 设计那节特别标注：v0 / Bolt / Figma 产出 Web 组件，**不能用于微信小程序**
- **`skills/multi-agent/SOP.md` 与 `orchestrator.md` 去 Hermes 化**（按 ADR-0005）
  - SOP 的架构图由「Hermes 主编 → 派发 → 3 个 Agent 并行」改为「静态技能树，一次推进一个角色」
  - `orchestrator.md` 由「主编 Agent：拆任务 / 派发 / review / 合并」改为「任务拆分：只产出任务卡，不调度任何进程」
  - 角色清单改为「收什么 → 交什么」的契约视角；去掉触发词表里的 `delegate_task` 等 harness 专属机制
- **`.claude/` `.codex/` `.cursor/` 下的三份技能副本改为符号链接**，统一指向 `skills/multi-agent/`——ADR-0002 已明确反对「每个工具复制一份」（内容漂移、维护成本翻倍）

### Fixed 修复
- `docs/10-platforms/wechat/miniprogram.md`：**纠正「个人主体 ❌ 流量主广告」的事实错误**——个人主体可开通流量主（UV 达到后台门槛即可），这是个人小程序唯一的原生变现通道，与 [phases/08-iterate](phases/08-iterate/README.md) 的变现路线保持一致；包大小限制改为引用官方分包文档并标注「以官方为准」（原文硬编码 20MB 已过时风险）
- `skills/README.md`「多 Agent 协作技能」小节：清掉与 ADR-0005 / 修订版 SOP 矛盾的残留表述（「Hermes 主编 + 角色 Agent 并行」「主编：拆任务、派发、Review、合并」），对齐为「角色 = 独立技能树、任务拆分只产出任务卡不调度」

### 计划中
- 补齐 `skills/` 目录中 15 个尚未创建的角色技能文件（见 [skills/README.md](skills/README.md) 状态标注，每个都附了可参考的外部实现）
- 是否补 `skills/testing/` 目录：现有 7 角色无「测试」。按 ADR-0005 的口径，它若补也应是一棵独立技能树，而非运行时里的审查子进程——待 ADR-0005 拍板后决定
- 考虑为 8 个 `phases/*/SKILL.md` 增加「反向触发（什么输入进来时不该用本技能）」字段——涉及 SKILL 格式变更，需先起草 ADR

### Decisions 决策（详见 [docs/decisions/](docs/decisions/)）

- [ADR-0005](docs/decisions/0005-roles-as-independent-skill-trees.md)（**提议中**）：角色 = 独立技能树，与任何 Agent 运行时解耦
  - 角色实现为 `skills/<角色>/` 下的自包含 Markdown 技能树，**不是**运行时进程、子代理，也不是某个 harness 的配置项
  - 角色之间靠输出契约衔接，不靠调用关系；技能文件内不出现任何 harness 特性（slash command、子代理调度、专属变量）
  - 分工清楚的判定标准：能说清「A 交给 B 的是什么文件、什么格式」；说不清就先补契约，不要先补技能
  - 放弃了「主编 + 子代理并行」（绑死 harness、review 压力推迟到最后一次性爆发）与「引入编排框架」（仓库从手册变成代码项目）两条路
  - 是 ADR-0002 在角色层的延伸：解耦对象从**文件格式**变成**执行方式**

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
