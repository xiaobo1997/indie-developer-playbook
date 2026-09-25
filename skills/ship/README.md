# 🚢 ship · 全平台一条龙发布

> **一句话**：你提供**代码 + 权限**，AI 完成**可行性预检 → 构建 → 部署 → 端到端验证 → 提交审核**，逐平台给证据报告。
> 设计原则：**先预检后动手**（不做一半发现做不了）、**API 优先**（浏览器只兜底）、**人工停机点最少且全部一次性**（见 ADR-0004 与 checklist-zero-to-prod 的敏感信息规则）。

## 文件结构

```
skills/ship/
├── README.md              ← 本页：总览与平台矩阵
├── SKILL.md               ← 主编排技能（喂给 AI 的入口）
├── feasibility-check.md   ← 可行性预检技能（硬门槛 go/no-go，开工前必跑）
├── ship-params.yaml       ← 全平台参数与权限清单模板（你填这个）
└── platforms/
    ├── web-h5.md          H5/Web：部署 → 域名 → E2E（无审核环节）
    ├── miniprogram.md     微信小程序：ci 上传 → API 提审 → API 发布（全 API）
    ├── minigame.md        微信小游戏：同小程序管线 + 软著硬门槛
    ├── ios.md             iOS：fastlane → TestFlight → 提审
    ├── android.md         Android：fastlane → Play 内测 → 提审（+国内商店现实）
    └── steam.md           Steam：steamcmd 构建 → 商店页 → 提审
```

## 平台 × 可行性矩阵（2026-09-22 预检，规则以各官方为准）

| 平台 | 自动化通道 | 人工停机点（一次性） | 硬费用 | 审核周期 | 到"提审"可行性 |
|---|---|---|---|---|---|
| **H5/Web** | CLI 全自动（见 [from-zero-to-k8s](../../docs/10-platforms/self-hosted/from-zero-to-k8s.md)） | 实名/充值/备案 | 服务器 ¥150-300/月 | 无审核 | ✅ 到上线全通 |
| **微信小程序** | miniprogram-ci + 管理 API（上传/提审/发布） | MP 扫码、下 ci 私钥、配 IP 白名单 | 免费（认证 ¥300/年 仅企业需要） | 1-3 天 | ✅ 全 API |
| **微信小游戏** | 同小程序 | 同上 + **软著**（1-3 个月，最大门槛） | 软著 ¥500-1000 | 1-3 天 | ⚠️ 卡软著周期 |
| **iOS App** | fastlane + ASC API（构建/TestFlight/提审） | 买账号、2FA、建 API Key | $99/年 | 1-3 天 | ✅ 全 API（需 macOS） |
| **Android(GP)** | fastlane supply + Play API | 注册、授权 service account | $25 一次性 | 数小时-7 天 | ⚠️ 新个人号需 12 测试员×14 天 |
| **Android(国内)** | 华为有 API；其余商店人工 | 各商店实名、软著常见 | 多数免费 | 1-7 天/店 | ⚠️ 人工协助层 |
| **Steam** | steamcmd 构建上传全自动 | 合作方注册、$100/app、税务面谈、点提审 | $100/app | 3-5 个工作日 | ✅ 构建 + 商店页就绪，提审按钮人来点 |

## 怎么发起

1. 复制 [ship-params.yaml](ship-params.yaml) 填好（只填你要的平台的段）
2. 对 AI 说：

```text
阅读 skills/ship/SKILL.md 并执行，参数在 ship-params.yaml：
先跑 feasibility-check.md 输出 go/no-go 报告再动手；
逐平台执行 platforms/*.md 管线；每步给验证证据；
人工停机点停下来给我指引；全程不许垫付、不许绕过任何平台审核。
```

3. 你会收到：**逐平台报告**（做了什么 / 证据 / 现在卡在哪 / 下一步谁做）。

## 与仓库其他部分的关系

- 服务器部署的完整版：[checklist-zero-to-prod](../../docs/10-platforms/self-hosted/checklist-zero-to-prod.md)（B-J 组细节）与 [from-zero-to-k8s](../../docs/10-platforms/self-hosted/from-zero-to-k8s.md)
- 平台知识库：[docs/10-platforms/](../../docs/10-platforms/README.md)；环节主线：[phases/06-release](../../phases/06-release/README.md)
- 格式与架构约束：[ADR-0002](../../docs/decisions/0002-agent-agnostic-skills.md)（技能六段）、[ADR-0005](../../docs/decisions/0005-roles-as-independent-skill-trees.md)（不绑运行时）
