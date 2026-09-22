# 环节 03 · 外部参考

> 收录政策见 [ADR-0004](../../docs/decisions/0004-external-references-policy.md)。

## 小程序组件库（选一个，别混用）

- [TDesign 小程序版](https://tdesign.tencent.com/) / [GitHub: Tencent/tdesign-miniprogram](https://github.com/Tencent/tdesign-miniprogram) —— 腾讯官方出品，组件覆盖全，文档好，新手首推。
- [Vant Weapp](https://github.com/youzan/vant-weapp) —— 有赞出品，社区案例多，轻量工具类小程序常用。
- [WeUI wxss](https://github.com/Tencent/weui-wxss) —— 微信官方视觉风格，和微信原生体验最统一。
- [微信小程序设计指南](https://developers.weixin.qq.com/miniprogram/design/) —— 官方规范：导航、字号、可点击区、配色可访问性（以官方为准）。

## Web 路线组件库（未来 Web/App 用）

- [shadcn/ui](https://github.com/shadcn-ui/ui) —— 事实标准：组件代码直接复制进项目、完全可控，且 AI 生成代码对它兼容性最好。
- [jnsahaj/tweakcn](https://github.com/jnsahaj/tweakcn) —— shadcn 可视化主题编辑器（[tweakcn.com](https://tweakcn.com) 免登录可用），点几下定一套配色。
- [heroicons](https://github.com/tailwindlabs/heroicons) —— Tailwind 官方图标库，免费。

## 灵感与抄作业

- [Mobbin](https://mobbin.com/) —— 海量真实 App 截图库，按页面类型（登录/列表/设置）检索，抄作业第一站（免费额度够用）。
- [Dribbble](https://dribbble.com/) —— 视觉灵感，看气质和配色，别照搬花哨布局。

## 单人设计工作流工具

- [Excalidraw](https://github.com/excalidraw/excalidraw)（[在线版](https://excalidraw.com/)）—— 手绘风白板，画低保真草图/页面流程最快的选择，开源免费。
- [Penpot](https://github.com/penpot/penpot) —— 开源 Figma 替代；真需要设计稿又不想要 Figma 订阅时用。
- [iconify](https://icon-sets.iconify.design/) —— 聚合数千套免费 SVG 图标集，按名字搜索即取即用。

## 工具生态（本环节能拿来用的）

> 列工具不是让你都用上，是让你知道**这个行业已经把哪些重复劳动做成了产品**。
> ⚠️ **v0 / Bolt / Figma 这类产出的是 Web 组件，不能用于微信小程序**（小程序是 WXML / WXSS）。做小程序请用上面的「小程序组件库」一组。
> 完整清单与实测状态见 [docs/03-ui/ui-reference-projects.md](../../docs/03-ui/ui-reference-projects.md) E 组。

| 工具 | 干什么 | 什么时候用 |
|---|---|---|
| [v0.dev](https://v0.dev) | 文本 → React / Tailwind 代码 | **首选**。产出是可改的代码，不是图 |
| [Bolt.new](https://bolt.new) | 文本 → 完整可跑的全栈应用 | 想看到能点、能用的东西时 |
| [Figma](https://www.figma.com) | 设计稿 + 原型 | **只做视觉确认，不做像素精修**（见 [ai-as-designer.md](../../docs/03-ui/ai-as-designer.md)） |
| [Motiff](https://www.motiff.com) | 国内 AI 设计工具，Figma 平替 | 中文场景、不想用英文界面 |
