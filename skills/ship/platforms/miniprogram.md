# 平台管线 · 微信小程序

> **全 API 路线**：`miniprogram-ci 上传 → 管理 API 提审 → API 轮询 → API 发布`，无浏览器依赖。
> 官方依据（2026-09-22 实测可达）：[ci 工具](https://developers.weixin.qq.com/miniprogram/dev/devtools/ci.html) · [提交审核 submitAudit](https://developers.weixin.qq.com/miniprogram/dev/OpenApiDoc/management/audit/submitAudit.html) · [查询审核状态 getAuditStatus](https://developers.weixin.qq.com/miniprogram/dev/OpenApiDoc/management/audit/getAuditStatus.html)。规则与限额以官方为准。

## 硬门槛（预检项）

| 检查项 | 要求 | 依据 |
|---|---|---|
| AppID 与主体 | 已注册；类目与实际功能匹配（医疗/教育等需前置资质） | MP 后台 |
| ci 上传密钥 | MP 后台「开发管理→开发设置」下载 `private.key`，只下载一次妥善保存 | 官方 ci 文档 |
| IP 白名单 | MP 后台配置调用方 IP（服务端 API 用 access_token 的前提），一次性 👤 | 官方 |
| AppSecret | 🔒 由人获取，AI 现场写入目标系统 | 敏感规则 |
| 代码可构建 | `npm install` + 构建（含云函数部署如用云开发）通过 | 实测 |
| 版本描述与截图 | 提审单需如实功能描述与页面截图 | 官方 |

## 自动化通道

| 动作 | 通道 |
|---|---|
| 上传代码为开发版本 | `miniprogram-ci` 的 `ci.upload`（Node API，需 appid + privateKey） |
| 生成体验版二维码 | `ci.preview`（二维码给真机扫码用） |
| 提交审核 | 服务端 API `POST /wxa/submit_audit`（access_token） |
| 查询审核状态 | `POST /wxa/get_auditstatus`（轮询） |
| 撤回审核 | `POST /wxa/withdraw_audit` |
| 审核通过后发布 | `POST /wxa/release` |

## 流水线

| # | 步骤 | 谁 |
|---|---|---|
| M-01 | 本地构建 + `ci.upload` 上传（记录版本号） | 🤖 |
| M-02 | `ci.preview` 出体验版二维码 → **交 👤 真机扫码** | 👥 |
| M-03 | E2E：真机核心链路 + 核心接口 200 | 👥 |
| M-04 | 组装提审参数（版本描述、页面截图清单、类目） | 🤖 |
| M-05 | API 提交审核，记录审核单号 | 🤖 |
| M-06 | 轮询 getAuditStatus；通过 → `release` 发布；被拒 → 取回原因，改后重提 | 🤖 |

## E2E 验收（交付标准）

- [ ] 体验版真机打开，核心链路走通（非开发者微信扫码）
- [ ] 核心接口在小程序内调用返回 200（后端域名已在 MP 后台合法域名列表）
- [ ] 提审返回成功（有审核单号）

## 人工停机点（一次性）

MP 管理员扫码登录、下载 ci 私钥、配 IP 白名单、拿 AppSecret。之后 M-01→M-06 可反复无人值守执行。

## 小游戏差异

管线完全相同（ci 支持 minigame）。**多一道硬门槛：[软著](https://www.ccopyright.com.cn/)（1-3 个月）+ 游戏类目资质**，预检不过不排期——这是小游戏提前 2-3 个月启动的原因，见 [minigame.md](minigame.md)。
