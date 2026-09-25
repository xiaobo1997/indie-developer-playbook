# 平台管线 · iOS App

> **全 API 路线**：fastlane + App Store Connect API——构建上传、TestFlight 分发、提审全部命令行完成。
> 官方依据（2026-09-22 实测可达）：[App Store Connect API](https://developer.apple.com/app-store-connect/api/) · [fastlane 文档](https://docs.fastlane.tools/) · [TestFlight](https://developer.apple.com/testflight/)。审核规则以官方为准。

## 硬门槛（预检项）

| 检查项 | 要求 | 说明 |
|---|---|---|
| macOS + Xcode | 必须真机 macOS 与最新稳定 Xcode | iOS 构建绕不开 |
| 开发者账号 | Apple Developer Program，**$99/年**（👤 购买） | [注册](https://developer.apple.com/programs/) |
| 2FA | 账号已开两步验证 | 建 API Key 的前提 |
| **ASC API Key** | App Store Connect → 用户和访问 → 集成，生成 `.p8` + Key ID + Issuer ID（👤 一次） | fastlane 的凭据 |
| Bundle ID | 已注册，与工程一致 | — |
| 代码可构建 | `xcodebuild archive`（或 pod install 后）成功 | 实测 |
| 上架材料 | 截图（各尺寸）、隐私政策 URL、描述、关键词 | fastlane deliver 可管 |
| 隐私标签 | App 隐私"营养标签"问卷 | 提审必填 |

## 自动化通道

| 动作 | 通道 |
|---|---|
| 签名与证书 | `fastlane match`（或手动 sigh） |
| 上传构建到 TestFlight | `fastlane pilot upload`（或 `xcrun altool`） |
| 分发给内测者 | TestFlight 内部组（API 管理） |
| 元数据与截图上传 | `fastlane deliver`（`metadata/` `screenshots/` 目录化） |
| **提交审核** | `fastlane deliver --submit_for_review`（ASC API） |
| 审核状态跟踪 | ASC API / `fastlane deliver` 查询 |

## 流水线

| # | 步骤 | 谁 |
|---|---|---|
| I-01 | archive + export ipa 成功（本地/CI macOS） | 🤖 |
| I-02 | `pilot upload` → TestFlight 构建处理完成 | 🤖 |
| I-03 | E2E：真机装 TestFlight 版，核心链路走通 | 👥 |
| I-04 | `deliver` 上传元数据 + 截图 + 隐私标签 | 🤖 |
| I-05 | `deliver --submit_for_review`，记录提审信息 | 🤖 |
| I-06 | 跟踪审核：被拒 → 按拒因修改 → 重提（Resolute 状态机在 ASC API） | 🤖 |

## E2E 验收（交付标准）

- [ ] TestFlight 真机安装、打开、核心链路通过
- [ ] 崩溃率：TestFlight 期间无已知必现崩溃
- [ ] 提审完成（ASC 后台状态"等待审核"）

## 人工停机点（一次性）

买账号、开 2FA、生成 `.p8` API Key（含权限授予）。此后 I-01→I-06 全自动可重复。

## 新账号注意

新开发者账号首次提审被拒率偏高（Guideline 2.1 信息不全、4.2 最小功能），预检报告里附常见拒因清单（见 [ios 平台文档](../../../docs/10-platforms/ios/ios-app.md) 的被拒原因表），提审前自查。
