# 一次开发，多端打包：iOS / Android / H5 / 小游戏 / Steam 一次性生成

> **先说清边界**（避免错误预期）：不存在"点一下出所有平台原生包"的魔法。"一次构建"的正确形态是：**一套代码 + 多端各一条构建命令 + CI 并行跑**。代码复用率 85-98%，但每端仍绕不开平台自己的关卡（iOS 签名与审核、Android 签名、小程序提审）。
> 🤖 **不想自己决策？**把产品情况交给 [skills/product/evaluate-platforms.md](../../skills/product/evaluate-platforms.md)——端组合推荐 + 路线 trade-off + 每端完整路径与总账，定案后 [skills/ship](../../skills/ship/README.md) 全流程执行。
> 与发布流程的衔接：构建只是 ship 管线的 I-01/A-01/S-01 步——出包之后的签名、上传、提审见 [skills/ship/](../../skills/ship/README.md)。

## 一、决策树：按你已有的代码形态选路线

这是唯一重要的决定，**选错了就要重写**：

```
你的代码现在是什么形态？
│
├─ 还没写 / 愿意换框架（最好情况）
│   ├─ 目标含 App（iOS+Android）为主 → **Flutter**（Dart）或 **Expo(React Native)**
│   ├─ 目标含微信小程序 → **Taro**（React 语法）或 **uni-app**（Vue 语法）
│   │     ↳ 一套代码同时出 小程序 + H5 + App
│   ├─ 纯 Web 为主，想要桌面/移动壳 → **Tauri 2.0**（Rust 壳，包小）或 **Capacitor**
│   └─ 是游戏 → **Cocos Creator**（小游戏+iOS+Android+Steam 最省力）
│
├─ 已有 Web 应用（React/Vue/Next）
│   ├─ 想要 iOS/Android App → **Capacitor** 包壳（改动最小）
│   └─ 想要桌面（Steam 路线）→ **Tauri** / Electron 包装
│
├─ 已有原生小程序代码
│   ├─ 想出 H5/App → **uni-app 迁移**（小程序语法最接近）或重写页面层
│   └─ 成本评估：页面层重写常比硬迁移快——先跑成本对比再动手
│
└─ 已有 Flutter / RN 代码 → 直接用其多端构建（见下表），别换
```

**本仓库主线提醒**（[ADR-0003](../decisions/0003-miniprogram-first.md)）：第一个产品是小程序，所以**默认答案 = Taro 或 uni-app**——它让"小程序 + H5 + 未来 App"共用一套代码。确定永远不做多端时才选原生小程序。

## 二、各路线的"一条命令"对照

| 路线 | 一套代码产出 | 构建命令（示例） | 官方文档 |
|---|---|---|---|
| **Flutter** | iOS + Android + Web + macOS/Win/Linux | `flutter build appbundle && flutter build ipa && flutter build web` | [iOS](https://docs.flutter.dev/deployment/ios) / [Android](https://docs.flutter.dev/deployment/android)（[中文站](https://flutter.cn)） |
| **Expo (RN)** | iOS + Android + Web | `eas build --platform all` ← **一条命令云端同时出双端包，没有 Mac 也能出 iOS** | [EAS Build](https://docs.expo.dev/build/introduction) |
| **Taro** | 小程序 + H5 + RN App | `npm run build:weapp && npm run build:h5`（RN 走 Taro RN） | [docs.taro.zone](https://docs.taro.zone/) |
| **uni-app** | 小程序 + H5 + App(iOS/Android) | HBuilderX 发行菜单 / `uni build -p mp-weixin` | [uniapp.dcloud.io](https://uniapp.dcloud.io/) |
| **Capacitor** | 把 Web 包成 iOS/Android | `npx cap add ios && npx cap add android && npx cap sync` 后各开原生构建 | [capacitorjs.com](https://capacitorjs.com/docs) |
| **Tauri 2.0** | 桌面(macOS/Win/Linux) + iOS/Android | `tauri build`（桌面）/ `tauri ios build` / `tauri android build` | [tauri.app](https://tauri.app/start/) |
| **Cocos Creator** | 小游戏 + iOS + Android + Web | 编辑器「构建发布」勾多平台；Steam 走 Web/桌面包装 | [cocos.com](https://www.cocos.com/creator) |

## 三、CI 一次 push，多端出包

**方案 A：GitHub Actions matrix**（各端并行跑，互不阻塞）：

```yaml
# .github/workflows/build-all.yml
name: build-all
on: { push: { tags: ["v*"] } }        # 打 tag 才触发，省额度
jobs:
  android:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-java@v4
        with: { distribution: temurin, java-version: "17" }
      - run: ./gradlew bundleRelease          # 出 AAB
      - uses: actions/upload-artifact@v4
        with: { name: android-aab, path: "**/*.aab" }
  web:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with: { node-version: 20 }
      - run: npm ci && npm run build          # 出 H5/静态包
      - uses: actions/upload-artifact@v4
        with: { name: web-dist, path: dist/ }
  ios:                                        # macOS runner（注意额度）
    runs-on: macos-14
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with: { node-version: 20 }
      - run: npm ci && npx cap sync ios       # Capacitor/Tauri-iOS 示例
      # 签名与上传走 fastlane，见 skills/ship/platforms/ios.md
```

**方案 B：Expo EAS**（RN 项目最省心）：`eas build --platform all --non-interactive`，云端队列并行出 iOS + Android，签名托管，配 CI 自动触发见 [官方 CI 指南](https://docs.expo.dev/build/building-on-ci/)。

**方案 C：fastlane 统一入口**（原生项目）：`fastlane ios ship` + `fastlane android ship` 两条 lane 串行/并行，签名到提审一把梭（管线见 [ship/platforms](../../skills/ship/README.md)）。

## 四、每端绕不开的现实（预检清单）

| 端 | 构建可全自动 | 但必须有 | 提审 |
|---|---|---|---|
| iOS | ✅（macOS 或 EAS 云构建） | **$99/年账号 + 签名**（EAS 可托管签名） | ASC 提审，[管线](../../skills/ship/platforms/ios.md) |
| Android | ✅ | $25 + 上传签名密钥 | Play 提审（新个人号有闭测要求） |
| 微信小程序/小游戏 | ✅（Taro/uni-app 编译） | AppID + ci 私钥 | 全 API 提审 |
| H5 | ✅ | 域名（国内需备案） | 无审核 |
| 桌面/Steam | ✅ | Steam 合作方 + $100/App | 商店页人工层 |

**诚实提醒**：跨端框架的"一套代码"里，**支付、推送、登录这类平台能力通常要写平台分支代码**（`if (platform === 'ios')`）——估算工作量时给这块留 10-15%。UI 层、业务逻辑、状态管理才是复用的大头。

## 五、独立开发者的推荐组合

1. **产品以小程序起步 + 未来要 App**：Taro（React 系）或 uni-app（Vue 系），先发小程序，App 端随时可出
2. **产品就是 App**：Expo（不买 Mac 也能出 iOS 包，EAS 免费档够个人用，额度以官方为准）或 Flutter（性能与生态上限更高）
3. **已有 Web 想快速有 App**：Capacitor 包壳，一周内能上 TestFlight
4. **游戏**：Cocos Creator 一套工程出小游戏 + 移动端 + Steam

## 相关

- 出包之后的发布：[skills/ship/](../../skills/ship/README.md)（预检 → 各平台管线 → 提审）
- 服务器端部署：[from-zero-to-k8s](../10-platforms/self-hosted/from-zero-to-k8s.md)
- 技术选型决策记录：[ADR-0003 小程序主线](../decisions/0003-miniprogram-first.md)
- 前端选型基础：[frontend.md](frontend.md)
