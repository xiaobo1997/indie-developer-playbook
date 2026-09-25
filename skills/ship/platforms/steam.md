# 平台管线 · Steam

> **构建全自动，提审半人工**：steamcmd 负责 depot 构建/上传（CLI 全自动）；合作方后台的商店页与"提交审核"按钮是人工层——AI 备好全部字段与素材包，人点最后几下。
> 官方依据（2026-09-22 实测可达）：[steamcmd 上传文档](https://partner.steamgames.com/doc/sdk/uploading) · [SteamCMD Wiki](https://developer.valvesoftware.com/wiki/SteamCMD)。政策以 Steamworks 为准。

## 硬门槛（预检项）

| 检查项 | 要求 | 说明 |
|---|---|---|
| 合作方账号 | Steamworks Partner 注册（个人可），身份/银行/**税务面谈**（👤 一次） | 提成结算需要 |
| **Steam Direct 费** | **$100/AppID**（可回收：收入达 $1000 后返还），（👤 支付） | 每 App 一笔 |
| AppID | 后台创建 App，拿到 AppID | — |
| 构建产物 | Windows/macOS/Linux 可执行包能跑（Web 游戏用 Electron/Tauri 包装为主流做法） | 实测 |
| 商店页素材 | 胶囊图、头图、截图 ≥5、预告片、描述——**有官方规格清单**，AI 按规格备齐 | 后台要求 |
| Steam 令牌 | 账号开手机令牌，steamcmd 首次 CLI 登录完成（👤 一次，配合一次性码） | 之后脚本可跑 |

## 自动化通道

| 动作 | 通道 |
|---|---|
| 构建 depot | `steamcmd +login +run_build_script app_build_<id>.vdf`（全自动） |
| 上传到 SteamPipe | 同上，构建输出带 manifest ID |
| 设置分支（public/beta） | build 脚本里配置 |
| 商店页与提审 | **合作方后台网页操作**（人工层，AI 给字段级指引） |

## 流水线

| # | 步骤 | 谁 |
|---|---|---|
| S-01 | 本地/CI 构建出可执行包，冒烟跑通 | 🤖 |
| S-02 | 写 `app_build_<id>.vdf` + `depot_build_<id>.vdf`，steamcmd 登录上传 | 🤖 |
| S-03 | 验证：后台构建页可见新 build，设为可玩分支 | 🤖（👤 核对） |
| S-04 | 商店页全部字段与素材：AI 生成字段包（描述文案见 [landing-page 技能](../../marketing/landing-page.md) 的诚实原则），👤 逐项粘贴 | 👥 |
| S-05 | 定价设置（👤）；**提交商店审核**（👤 点按钮） | 👤 |
| S-06 | 审核跟踪（Valve 通常 3-5 个工作日，以官方为准）；反馈问题 → AI 改素材/文案 → 重提 | 👥 |

## E2E 验收（交付标准）

- [ ] steamcmd 上传成功（有 manifest ID 与后台构建记录）
- [ ] 构建在 Steam 客户端"开发者模式/自家 App"可启动
- [ ] 商店页素材齐全进入审核（审核中状态）

## 现实建议

- Steam 是"游戏"商店：确认产品形态（Web 小游戏要包装成桌面可执行），预检时测"包装后可跑"这一步
- 首发定价与成就/云存档等 Steam 功能按官方 [store page 文档](https://partner.steamgames.com/doc/store/application/home) 核对，以官方为准
- $100 是每 App 一笔——预检报告里把"是否真的要上 Steam"和这笔钱一起摆给用户决策
