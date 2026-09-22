# 环节 06 · 外部参考

> 收录政策见 [ADR-0004](../../docs/decisions/0004-external-references-policy.md)。审核规则、时效、限额均以官方文档为准。

## 微信官方：发布与审核

- [代码上传（开发者工具 / miniprogram-ci）](https://developers.weixin.qq.com/miniprogram/dev/devtools/ci.html) —— 上传代码、生成体验版的官方文档；提交审核与发布在 [mp.weixin.qq.com](https://mp.weixin.qq.com/) 后台「版本管理」完成。
- [小程序运营规范](https://developers.weixin.qq.com/miniprogram/product/) —— 被拒原因的权威清单：诱导分享、类目不符、内容违规都在这。
- [用户隐私保护指引](https://developers.weixin.qq.com/miniprogram/dev/framework/user-privacy/) —— 提审前必配。
- [服务类目与资质](https://developers.weixin.qq.com/miniprogram/product/) —— 类目资质要求查询（注册/类目管理后台内操作）。
- [运维中心说明（mp 后台）](https://mp.weixin.qq.com/) —— JS 错误、性能、接口失败的免费监控入口。

## 自建后端部署（Web 路线/进阶）

- [Vercel](https://vercel.com/home) —— Web 前端/Next.js 免费起步部署。
- [Cloudflare Pages](https://pages.cloudflare.com/) —— 静态站 + Workers，国内可直连的边缘部署选项。
- [Sentry Self-Hosted](https://github.com/getsentry/self-hosted) —— 自建错误监控（有服务器时）；小程序优先用微信自带运维中心。

## 监控与备份（最小可用）

- [Uptime Kuma](https://github.com/louislam/uptime-kuma) —— 开源拨测监控：给你的 Web 服务/API 加"挂了就通知"，一人项目够用。
- [PocketBase 备份思路参考](https://github.com/pocketbase/pocketbase) —— 单文件数据库的定时快照是最简备份模型；云开发用其自带导出，以官方文档为准。

## 本仓库相关

- [skills/devops/deploy-checklist](../../skills/devops/deploy-checklist.md) ✅ —— 通用部署检查清单
- [docs/06-devops/devops.md](../../docs/06-devops/devops.md) —— 部署运维角色详解

## 工具生态（本环节能拿来用的）

> 列工具不是让你都用上，是让你知道**这个行业已经把哪些重复劳动做成了产品**。价格与可用性以官网为准（实测 2026-09-21 均可访问）。
> 小程序发布走微信 MP 后台（见上），下面这组主要服务 **自建后端 / Web 路线**。

| 工具 | 干什么 | 什么时候用 |
|---|---|---|
| [Vercel](https://vercel.com) | 前端部署 | Next.js 项目最省事的选择，推代码即部署 |
| [Netlify](https://www.netlify.com) | 静态站点部署 | 落地页、文档站；表单和重定向开箱可用 |
| [Cloudflare Pages](https://pages.cloudflare.com) | 静态站点托管 | 想省成本、要全球 CDN 时 |
| [Sentry](https://sentry.io) | 错误监控 | **上线当天就该就位**；小程序另有免费的微信运维中心 |
