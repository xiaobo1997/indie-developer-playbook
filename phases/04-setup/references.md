# 环节 04 · 外部参考

> 收录政策见 [ADR-0004](../../docs/decisions/0004-external-references-policy.md)。平台规则以官方文档为准。

## 微信小程序官方（本环节主战场）

- [小程序注册入口（mp.weixin.qq.com）](https://mp.weixin.qq.com/) —— 注册账号拿 AppID，选主体类型前先读「小程序主线注意点」第 1 条。
- [开发框架文档](https://developers.weixin.qq.com/miniprogram/dev/framework/) —— 目录结构、配置、页面生命周期的权威来源。
- [目录结构规范](https://developers.weixin.qq.com/miniprogram/dev/framework/structure.html) —— 官方推荐的项目结构，脚手架照此来。
- [微信开发者工具下载](https://developers.weixin.qq.com/miniprogram/dev/devtools/download.html) —— 选稳定版（Stable）。
- [云开发文档](https://developers.weixin.qq.com/miniprogram/dev/wxcloud/) —— 数据库/云函数/存储，个人开发者免运维后端的默认答案。
- [小程序开发助手](https://github.com/wechat-miniprogram) —— 微信官方 GitHub 组织：miniprogram-simulate（组件测试）、官方示例与扩展组件库都在这。

## 多端框架（确认要多端再看）

- [Taro](https://github.com/NervJS/taro) / [文档](https://docs.taro.zone/) —— 京东出品，React 语法编译到小程序/:H5/RN。
- [uni-app](https://github.com/dcloudio/uni-app) —— DCloud 出品，Vue 语法，生态在国内多端场景最广。

## 自建/BaaS 后端（Web 路线或进阶）

- [Supabase](https://github.com/supabase/supabase) —— 开源 Firebase 替代：Postgres + 认证 + 存储 + 实时，免费额度起步。
- [PocketBase](https://github.com/pocketbase/pocketbase) —— 单二进制后端（数据库+认证+文件+实时），一人项目的极简自托管方案。

## Web 路线脚手架（备查）

- [create-t3-app](https://github.com/t3-oss/create-t3-app) —— Next.js + tRPC + Prisma + Tailwind，一条命令的全栈起点。
- [Open SaaS by Wasp](https://github.com/wasp-lang/open-saas) —— 免费开源 SaaS 套件：含 Stripe 支付与管理后台。
- [vercel/nextjs-subscription-payments](https://github.com/vercel/nextjs-subscription-payments) —— Vercel 官方 Stripe 订阅示例，轻量易改。

## 给 AI 的项目说明

- [AGENTS.md 开放约定](https://agents.md) —— 为什么放一份 AGENTS.md 在项目根目录，以及主流代理如何读它。
- [本仓库根目录 AGENTS.md](../../AGENTS.md) —— 直接复制改成你的项目版。
