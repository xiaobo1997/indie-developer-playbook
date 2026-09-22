# 环节 08 · 外部参考

> 收录政策见 [ADR-0004](../../docs/decisions/0004-external-references-policy.md)。变现规则以官方文档为准。

## 数据分析

- [微信小程序后台（mp.weixin.qq.com）](https://mp.weixin.qq.com/) —— 「统计/We 分析」：来源、留存、页面路径、性能与错误，个人项目免费够用。
- [订阅消息能力](https://developers.weixin.qq.com/miniprogram/dev/framework/open-ability/subscribe-message.html) —— 合规的召回通道（用户主动订阅制，以官方为准）。
- [Umami](https://github.com/umami-software/umami) —— 开源轻量网站分析，自托管一条 Docker 命令跑起（Web 路线）。
- [Plausible Analytics](https://github.com/plausible/analytics) —— 隐私友好的开源分析，两人做到百万美元 ARR 的样板（Web 路线；本仓库有[案例复盘](../../examples/plausible/README.md)）。

## 变现与定价

- [微信支付商户平台](https://pay.weixin.qq.com/) —— 商户号、费率、类目要求的权威来源（需企业/个体户主体）。
- [小程序流量主说明](https://developers.weixin.qq.com/miniprogram/product/) —— 广告组件开通条件与规范（门槛以后台/官方文档为准）。
- [examples/plausible 复盘](../../examples/plausible/README.md) —— "先开源涨口碑、再订阅变现"的路线样本。
- [Indie Hackers](https://www.indiehackers.com/) —— 搜 "monetization"：大量一人产品的首个付费用户复盘。

## 迭代方法

- [Shape Up（Basecamp）](https://basecamp.com/shapeup) —— 六周循环 + Shipped it 节奏，本环节"每周一个可见变化"的源头方法论。
- [templates/changelog-template.md](../../templates/changelog-template.md) —— 本仓库的版本记录模板。
- [Keep a Changelog](https://keepachangelog.com/zh-CN/1.1.0/) —— CHANGELOG 书写规范。

## 数据回溯：失败都发生在哪一环

- [references/open-source-repos.md](../../references/open-source-repos.md) —— 本仓库汇总的开源资料，第七节把 101 场创始人访谈的失败模式逐条映射到了本仓库的环节。与本环节直接相关的三条：**现金流断裂（提及 13 次）**、**失败后拒绝转向（10 次）**、**第一天就不收费**。对策就是本环节的周复盘 + CHANGELOG + 必要时写 ADR——让转向变得便宜。

## 留存与增长的度量常识

- [Dave McClure 的 AARRR 模型](https://www.indiehackers.com/)（500 Startups 经典框架，站内多篇实践讨论可检索） —— 获客/激活/留存/收入/推荐五步漏斗，定位你卡在哪一步。
- [docs/13-cases](../../docs/13-cases/case-studies.md) —— 本仓库案例集中的数据与变现复盘。

## 工具生态（本环节能拿来用的）

> 列工具不是让你都用上，是让你知道**这个行业已经把哪些重复劳动做成了产品**。价格与可用性以官网为准（实测 2026-09-21 均可访问）。

| 工具 | 干什么 | 什么时候用 |
|---|---|---|
| [Plausible](https://plausible.io) | 隐私友好统计 | 不想被 Cookie 弹窗折磨时；界面本身就是极简 UI 教材 |
| [Umami](https://umami.is) | 开源统计，可自部署 | 想零成本、数据自己掌握时 |
| [Crisp](https://crisp.chat) | 客服聊天 | 站内收反馈；免费额度够单人项目 |
| [Stripe](https://stripe.com) | 收款 | 海外收款的事实标准（国内用微信支付 / 支付宝） |
