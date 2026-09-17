# 环节 05 · 外部参考

> 收录政策见 [ADR-0004](../../docs/decisions/0004-external-references-policy.md)。

## 小程序开发官方文档（开发期常开）

- [开发框架](https://developers.weixin.qq.com/miniprogram/dev/framework/) —— WXML/WXSS/组件/API 的权威参考。
- [分包加载](https://developers.weixin.qq.com/miniprogram/dev/framework/subpackages.html) —— 主包体积超限时的解法（限额以官方为准）。
- [用户隐私保护指引](https://developers.weixin.qq.com/miniprogram/dev/framework/user-privacy/) —— 采集用户信息接口的合规要求。
- [wx.requestPayment（支付 API）](https://developers.weixin.qq.com/miniprogram/dev/api/payment/wx.requestPayment.html) —— 需企业主体 + 商户号；个人主体规划变现时先读 [phases/08-iterate](../08-iterate/README.md)。
- [云开发](https://developers.weixin.qq.com/miniprogram/dev/wxcloud/) —— 数据库、云函数、云存储、HTTP API。

## 测试

- [miniprogram-simulate](https://github.com/wechat-miniprogram/miniprogram-simulate) —— 微信官方的小程序组件测试库（配合 Jest 用）。
- [Jest](https://github.com/jestjs/jest) —— 纯逻辑单元测试的默认选择；别一上来就上端到端全家桶。

## 值得通读的单人/小团队项目源码（学"克制"）

- [PocketBase](https://github.com/pocketbase/pocketbase) —— 一人写的完整后端：单二进制、零依赖部署。读它的目录结构与 API 设计。
- [Uptime Kuma](https://github.com/louislam/uptime-kuma) —— 一人写的 50k+ star 监控产品：功能克制、架构朴素的正面教材。
- [Listmonk](https://github.com/knadh/listmonk) —— 一人写的生产级邮件系统，代码干净、文档完整。

## AI 结对开发

- [AGENTS.md 开放约定](https://agents.md) —— 项目里给 AI 立规矩的标准位置。
- 本仓库 [docs/decisions/0002](../../docs/decisions/0002-agent-agnostic-skills.md) —— SKILL 六段结构与输出契约的约定；任务卡格式对齐它。
