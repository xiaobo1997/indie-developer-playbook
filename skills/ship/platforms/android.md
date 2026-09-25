# 平台管线 · Android（Google Play 为主，国内商店另说）

> **全 API 路线**：fastlane supply + Google Play Developer API——AAB 上传、内测轨道、提审命令行完成。
> 官方依据：[Play Developer API](https://developers.google.com/android-publisher)（一方文档；本机网络 000 仅收录备注）· [fastlane supply](https://docs.fastlane.tools/actions/supply/)。**新个人账号测试要求**（12 测试员连续 14 天闭测，新号才能上生产）以 [官方政策](https://support.google.com/googleplay/android-developer/answer/1410048) 为准——这是预检最大变数。

## 硬门槛（预检项）

| 检查项 | 要求 | 说明 |
|---|---|---|
| 开发者账号 | Google Play Console，**$25 一次性**（👤） | — |
| **Service Account** | Play Console → 设置 → API 权限：关联项目、建服务账号、授权 JSON（👤 一次） | fastlane 凭据 |
| 代码可构建 | `./gradlew bundleRelease` 产出 AAB 成功 | 实测 |
| 签名 | 上传密钥/应用签名方案配置好（Play App Signing） | — |
| 上架材料 | 包名、商店截图、描述、隐私政策 URL、数据安全表单 | 提审必填 |
| 新号测试要求 | 个人账号（2023-11 后注册）需闭测达标 | **预检必查**，blocked 就先攒测试员 |

## 自动化通道

| 动作 | 通道 |
|---|---|
| AAB 上传 | `fastlane supply` / Play API `edits`（上传到 internal 轨道） |
| 分发给测试员 | internal/closed testing 轨道（API 管理） |
| 商店元数据 | `fastlane supply --metadata_path`（目录化） |
| **提交审核（生产）** | supply/promote 到 production 轨道 → 进入审核；或用 Track Release API |
| 状态跟踪 | Play Console API 查询发布状态 |

## 流水线

| # | 步骤 | 谁 |
|---|---|---|
| A-01 | `bundleRelease` 成功，AAB 就绪 | 🤖 |
| A-02 | 上传 internal 轨道，加测试员 | 🤖 |
| A-03 | E2E：真机 opt-in 内测链接安装、核心链路走通 | 👥 |
| A-04 | 元数据 + 数据安全表单填好（supply 上传） | 🤖 |
| A-05 | 提升 production 轨道提交审核（记录版本与状态） | 🤖 |
| A-06 | 审核跟踪；被拒按原因修改重提 | 🤖 |

## E2E 验收（交付标准）

- [ ] 内测真机安装、打开、核心链路通过
- [ ] 提审进入审核状态（或新号闭测期数据达标在案）

## 国内 Android 商店（现实层）

- **华为 AppGallery 有 Publishing API**，可自动化程度较高；小米/OPPO/vivo/应用宝等**无完善公开 API**，人工控制台提审为主——AI 给逐字段填写包（图标/截图/软著号/隐私政策），人逐店提交
- 国内上架普遍要求**软著**（同小游戏门槛）；工程量 = 商店数 × 提审流程，建议首发 1-2 家跑通再铺
- 详单见 [android 平台文档](../../../docs/10-platforms/android/android-app.md)
