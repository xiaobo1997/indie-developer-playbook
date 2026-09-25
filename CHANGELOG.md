# 变更日志（Changelog）

本仓库遵循 [Keep a Changelog](https://keepachangelog.com/zh-CN/1.1.0/) 格式，版本号遵循语义化版本（SemVer）。

> **看什么变了，为什么变**：变更日志只记录「改了什么」；「为什么这么改」记录在 [docs/decisions/](docs/decisions/)（决策记录 ADR）。

## [Unreleased]

### Added 新增（2026-09-22 · 做什么端评估技能）
- **`skills/product/evaluate-platforms.md`**：做什么端评估技能（评估 → 选端 → 交 ship 执行的闭环第一环）
  - 端价值表速查：7 个端的触达/变现/费用/周期门槛/个人主体约束
  - 跨端路线 trade-off 表：Taro/uni-app/Expo/Flutter/Capacitor/Tauri/Cocos/原生双端，每条含「得到/付出/什么时候会后悔」
  - 输出契约：P0 首发 ≤2 端的组合推荐、路线选型与放弃项、每端完整路径（构建→打包→发布→部署→提审，引用 ship 管线不复制）、总账（费用+里程碑日期）、风险与前置
  - 下游衔接：定案 → phases/02-spec 裁范围 → skills/ship/feasibility-check → ship 全流程执行；已有明确端与路线时反向触发跳过本技能
- `skills/README.md` 产品节挂接；`docs/04-frontend/multi-platform-build.md` 顶部挂接评估技能入口

### Added 新增（2026-09-22 · 多端一次构建指南）
- **`docs/04-frontend/multi-platform-build.md`**：一次开发、多端打包（iOS/Android/H5/小程序/小游戏/Steam）
  - 按现有代码形态的路线决策树（未开工/已有 Web/已有原生小程序/游戏四分支）；默认答案对齐 ADR-0003：Taro/uni-app 覆盖「小程序+H5+未来 App」
  - 七条路线「一条命令」对照表（Flutter / Expo EAS（无 Mac 出 iOS 包）/ Taro / uni-app / Capacitor / Tauri 2.0 / Cocos）
  - CI 一次 push 多端出包：GitHub Actions matrix 完整示例（android/web/ios 三 job 并行）+ EAS + fastlane 统一入口
  - 各端现实表：构建可全自动 vs 必须有的账号/签名/费用；平台分支代码预留 10-15% 工作量的诚实提醒
  - 与 skills/ship 各平台管线衔接（构建是 I-01/A-01/S-01 步）
  - 新增外链已验证（2026-09-22）：docs.flutter.dev、docs.expo.dev、capacitorjs.com、tauri.app、cocos.com、flutter.cn 均 200

### Added 新增（2026-09-22 · 全平台一条龙发布技能）
- **`skills/ship/`**（10 个文件）：「代码 + 权限」进，「逐平台提审 + 端到端验证证据」出
  - `SKILL.md`：主编排技能（先预检后动手；构建验证 → 逐平台管线 → E2E 收尾 → 总报告）；不垫付、不代持、不绕审核
  - `feasibility-check.md`：可行性预检技能（硬门槛逐项核查 + 代码可构建性实测；blocked 必附补办路径：缺什么/去哪办/多久/多少钱）
  - `ship-params.yaml`：全平台参数与权限清单模板（🔒 敏感项现场处理规则）
  - `platforms/` 六平台管线（各含硬门槛表 / 自动化通道 / 流水线 / E2E 验收 / 人工停机点）：web-h5（无审核）、miniprogram（**全 API**：ci.upload → submit_audit → get_auditstatus → release）、minigame（同管线 + 软著 1-3 个月硬门槛，建议并行启动）、ios（fastlane + ASC API 全自动到提审）、android（fastlane supply；新个人号 12 测试员×14 天闭测变数；国内商店人工协助层）、steam（steamcmd 构建全自动；$100/AppID + 商店页人工层）
  - 关键 API 通道已实测（2026-09-22）：微信 submitAudit/getAuditStatus（200）、ASC API、fastlane、steamcmd、Steamworks 上传文档均可达；Google 两个文档站本机 000（一方站点照常收录）
- `skills/README.md` 新增「🚢 一条龙发布」节；`phases/06-release/README.md` 挂接 ship 技能

### Changed 变更（2026-09-22 · API 优先执行架构）
- `checklist-zero-to-prod.md` 新增「自动化通道：能 API 的绝不开浏览器」：执行架构定为 CLI > OpenAPI > 浏览器兜底（规避 React 控制台的浏览器自动化脆弱性）
  - 通道表：aliyun/tccli（ECS 买卖、安全组、DNS、镜像仓）、gh（仓库/Secrets/Actions）、kubectl/helm（集群内）、微信小程序管理 API（modify_domain 改合法域名，前置人配 IP 白名单）
  - 真正绕不开人的收敛为一次性 4 件：账号实名充值 / ICP 备案 / MP 扫码+白名单 / 域名实名；此后建机到上线全程 AI 无头执行
  - A-02/A-03（RAM 子账号+AK）、B-02（API 建机 RunInstances）、D-04（安全组 API）、H-01/H-03/H-05（DNS 与小程序域名走 API）相应改写；停机点三类改四类
  - 新增外链已验证（2026-09-22）：微信 modify-domain 文档、aliyun CLI 安装、tccli 文档、ECS RunInstances 文档

### Changed 变更（2026-09-22 · CI 平台选型）
- `from-zero-to-k8s.md` 阶段 10 新增「代码仓库与 CI 平台怎么选」：GitHub+Actions（默认）/ GitLab.com / GitLab CE 自托管（4G+ 内存，别与 K3s 同机）/ 国内一站式（云效、CODING、Gitee Go）对比表；给出跨境部署的解法——自托管 runner 装在部署服务器（deploy job 本地执行，零延迟不限时长）

### Added 新增（2026-09-22 · 人机协作部署清单）
- **`docs/10-platforms/self-hosted/checklist-zero-to-prod.md`**：部署配置清单 + 可执行 CheckList（人机协作版）
  - 参数表模板（deploy-params.yaml，产品/云/域名/镜像仓/微信/告警/预算，🔒 敏感项留空现场处理）
  - A-J 十组共 44 项 CheckList，逐项标注执行者（👤人：支付/实名/扫码/备案；🤖AI：终端/配置/浏览器代操作；👥协作）+ 完成标准
  - 敏感信息规则（密码在服务器上生成入 Secret、不经聊天不经 Git）；断点续跑（「从 D-03 继续」）；发起执行的 prompt 模板（含预算上限与破坏性命令保护）
  - 与 from-zero-to-k8s.md 分工：那篇是"为什么与怎么做"，本篇是"照着跑的执行骨架"

### Added 新增（2026-09-22 · 部署全流程指南）
- **`docs/10-platforms/self-hosted/from-zero-to-k8s.md`**：从购买云服务器到用户用上 App 的完整按序路线（10 阶段）
  - 购买决策（国内/海外、规格 4C8G 起步）→ 域名 + ICP 备案 → 服务器初始化（SSH/ufw/fail2ban/内核参数）→ 前后端 Docker 打包与镜像仓库 → K3s 安装（单机实用主义，注明与 kubeadm 的取舍）→ 中间件（bitnami postgresql/redis，含托管 RDS 决策表）→ 配置初始化（Secret/ConfigMap/迁移 Job/探针/资源限额）→ 前后端 Deployment + Ingress + cert-manager HTTPS → 域名解析与白名单收口（安全组核对 + 小程序合法域名）→ GitHub Actions CI/CD 与上线验收清单
  - 含全程成本参考表、新手 10 坑（海外服务器做小程序、数据库裸奔公网、latest tag 无法回滚等）
  - 新增外链已验证（2026-09-22）：docs.k3s.io、kubernetes.io、helm.sh、min.io、aliyun.com、cloud.tencent.com、docs.docker.com；cert-manager.io / artifacthub.io / docker.io / k3s.io 本机网络不通（000，Cloudflare 系），知名官方站点照常收录

### Added 新增（2026-09-22 · 角色层补齐）
- **16 个角色技能补齐**（skills/ 全部 ✅，共 19 个）：product/prd-template、product/user-story、ui/design-review、ui/component-pattern、frontend/tailwind-setup、frontend/responsive-checklist、backend/api-design、backend/db-schema、devops/incident-response、devops/backup-strategy、marketing/launch-plan、marketing/landing-page、marketing/content-calendar、launch/product-hunt、launch/twitter-launch、launch/press-kit
  - 全部遵循 ADR-0002 六段结构与输出契约；「先读 docs/XX」渐进披露；边界节含反向触发说明
  - 与环节技能分层：环节 SKILL 管"什么时候做什么"（策略），角色技能管"单件事怎么做好"（执行）；重叠处已相互标注分工（如 launch-plan 渠道执行卡 vs phases/07 的策略层）
- **12 个角色目录自包含 README**（对齐 docs/03-ui 样板的七要素：职责边界/独立流程/收交契约/真实参考/新手坑/文件索引/带走清单）：01-mindset、02-product、04-frontend、05-backend、06-devops、07-marketing、08-support、09-finance、11-workflow、12-tools、13-cases
- **docs/10-platforms/README.md**：平台选择索引（6 平台对比 + 平台×环节接缝表）
- 新增外部链接均经实测验证（可访问性 2026-09-22；notion.so、tailwindcss.com、react.dev、vuejs.org、ui.shadcn.com、heroicons.com、iconify.design、crisp.chat、stripe.com、paddle.com、lemonsqueezy.com、hoppscotch.io、vitest.dev、playwright.dev、uptimerobot.com、uniapp.dcloud.io 等）；shipfa.st 与 agentskills.io 本机网络不通（000），为知名一方站点，浏览器可访问，照常收录

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
- ~~补齐 `skills/` 目录中尚未创建的角色技能文件~~（2026-09-22 已全部补齐 ✅，skills/README.md 状态表全绿）
- 各角色 README 目前是自包含入口，可学 03-ui 的做法继续加深度文档（如 frontend 加一篇「AI 结对验收」）
- 是否补 `skills/testing/` 目录：现有 7 角色无「测试」。按 ADR-0005 的口径，它若补也应是一棵独立技能树，而非运行时里的审查子进程——待 ADR-0005 拍板后决定
- 考虑为 8 个 `phases/*/SKILL.md` 增加「反向触发（什么输入进来时不该用本技能）」字段——涉及 SKILL 格式变更，需先起草 ADR（角色技能已在「边界」节以文字形式落地反向触发）

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
