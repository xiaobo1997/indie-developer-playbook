# 10 · 平台专项索引

> 平台不是角色：同一套角色技能（PM/UI/前端/后端/DevOps/营销/客服）在不同平台上打法不同。本索引帮你**选平台**，并把平台专属规则接回环节主线。
> 每个平台目录内是该平台的完整开发指南（人读）；环节执行时的平台注意事项在各环节 README 的「小程序主线注意点」。

## 怎么选平台

| 你的情况 | 选 | 理由与门槛 |
|---|---|---|
| 国内用户 + 想最快上线第一个产品 | **[微信小程序](wechat/miniprogram.md)** | 免服务器起步（云开发）、分发内置；个人主体免注册费但不能开支付 |
| 想顺手做小游戏 | [微信小游戏](wechat-game/mini-game.md) | Canvas/WebGL + Cocos；需软著，内购抽成见文档 |
| 海外用户 + 订阅制 SaaS | [Web SaaS](web-saas/saas.md) | 全球触达、Stripe 收款；冷启动靠内容/PH |
| iOS 付费 App | [iOS](ios/ios-app.md) | $99/年 + macOS + 审核严；付费意愿最高 |
| Android 全球分发 | [Android](android/android-app.md) | $25 一次性；国内多商店各自上架 |
| 数据主权 / 成本敏感 | [自建服务器](self-hosted/self-hosted.md) | 最自由也最累：HTTPS + ICP 备案 + 运维全自担 |

**默认答案**（本仓库主线，见 [ADR-0003](../decisions/0003-miniprogram-first.md)）：第一个产品做微信小程序，Web SaaS 作为第二主线备用。

## 平台 × 环节的接缝

| 环节 | 平台差异看哪 |
|---|---|
| 04 脚手架 | 各平台目录的「项目结构 / 技术栈选择」节 |
| 06 上线 | 小程序审核与备案 / iOS 审核被拒表 / 国内 Android 多商店流程——各目录「上线流程」节 |
| 08 变现 | 小程序主体限制（个人可开流量主、不可微信支付）/ iOS 虚拟支付 / Google Play 抽成——各目录「变现/支付」节 |

## 目录与状态

| 目录 | 平台 | 状态 |
|---|---|---|
| [wechat/](wechat/miniprogram.md) | 微信小程序 | ✅ 完整（主线） |
| [wechat-game/](wechat-game/mini-game.md) | 微信小游戏 | ✅ 完整 |
| [ios/](ios/ios-app.md) | iOS App | ✅ 完整 |
| [android/](android/android-app.md) | Android App | ✅ 完整 |
| [web-saas/](web-saas/saas.md) | Web SaaS | ✅ 完整 |
| [self-hosted/](self-hosted/self-hosted.md) | 自建服务器 | ✅ 完整 |

规则类内容（审核、抽成、包体积、备案）随平台政策变化，一律以各目录引用的官方文档为准（[ADR-0004](../decisions/0004-external-references-policy.md)）。

## 相关

- [phases/README.md](../../phases/README.md) —— 行动主线（平台差异是支线）
- [docs/README.md](../README.md) —— 角色知识库总索引
