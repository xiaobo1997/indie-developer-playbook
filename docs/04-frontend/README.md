# 04 · 前端角色目录

> 前端角色拿到 UI 角色的 `pages.md` 与 `design-tokens.md`，把页面变成可运行、可验收的代码。本目录让你独立跑完这个交接，AI 写码时代你的核心工作是**验收**而不是敲字。
> 对应环节：[环节 05 开发](../../phases/05-build/README.md)（前端部分）。

## 这个角色负责什么

| 负责 | 不负责 |
|---|---|
| 按页面清单实现界面与交互 | 定页面长什么样（UI 角色） |
| 组件选型落地、状态管理、接口对接 | 定接口契约（后端角色的 `api-design`） |
| 自测三态（空/载/错）+ 多端适配 | 决定功能做不做（PM 角色） |

**边界判定**：产出物是"能跑的页面 + 验收步骤"；产出物是"接口文档"或"布局设计"就越界了。

## 独立跑完「前端开发」的流程

| 步 | 做什么 | 产出 | 耗时 |
|---|---|---|---|
| 1 | 核对输入：`pages.md` + `design-tokens.md` + API 契约齐不齐 | 缺口清单（缺就先要） | 15 分钟 |
| 2 | 按 UI 角色的组件映射建页面骨架 | 页面文件 + 路由/Tab 结构 | 半天 |
| 3 | 逐页实现：先空态 → 再数据态 → 再交互 | 每页一个 commit | 按任务卡 |
| 4 | 三态自测 + 多端检查（小程序真机三档） | 自测结果记录 | 每页 15 分钟 |
| 5 | 交验收：按任务卡完成标准逐条给证据 | 验收步骤（在哪个页面点哪看什么） | 10 分钟/页 |

**技术选型速查**：小程序原生（默认）/ Taro（要多端，[docs.taro.zone](https://docs.taro.zone/)）/ uni-app（Vue 系，[uniapp.dcloud.io](https://uniapp.dcloud.io/)）；Web 用 Next.js/Vue + Tailwind + [shadcn/ui](https://ui.shadcn.com/)。选型详表见 [frontend.md](frontend.md)。

## 收什么 → 交什么

| 上游交给我（UI 角色 / 环节 03） | 我交给下游（DevOps 角色 / 环节 06） |
|---|---|
| `pages.md`（页面/职责/组件映射） | 可运行的版本（主分支随时可跑） |
| 关键页线框（含空/载/错三态） | 每任务的验收步骤 |
| `design-tokens.md`（色/字/距/圆角） | 遗留问题清单（发现但没修的） |
| ← API 契约（后端角色的 `api-design` 产出） | |

## 真实参考（不用凭空想象）

- 小程序组件库：[TDesign](https://tdesign.tencent.com/)（腾讯官方）/ [Vant Weapp](https://github.com/youzan/vant-weapp) / [WeUI](https://github.com/Tencent/weui-wxss)——选定一个，别混用
- Web 组件：[shadcn/ui](https://ui.shadcn.com/) + [heroicons](https://heroicons.com/)；样式 [Tailwind](https://tailwindcss.com/)
- 基础参考：[MDN](https://developer.mozilla.org/)、[React](https://react.dev/)、[Vue](https://vuejs.org/)
- 小程序官方：[开发框架](https://developers.weixin.qq.com/miniprogram/dev/framework/)、[分包加载](https://developers.weixin.qq.com/miniprogram/dev/framework/subpackages.html)（以官方为准）
- 本仓库技能：[tailwind-setup](../../skills/frontend/tailwind-setup.md) ✅ / [responsive-checklist](../../skills/frontend/responsive-checklist.md) ✅

## 新手最容易踩的坑

| 坑 | 为什么会踩 | 怎么避 |
|---|---|---|
| 只在模拟器/浏览器看 | 真机问题（手势/字体/安全区）看不见 | 每页完成即在真机过一遍；三档机型 |
| 只做"有数据"的样子 | 空态是用户第一屏 | 三态是验收标准，不是锦上添花 |
| 生成-粘贴-祈祷 | 信任 AI 不验收 | 验收三步（跑起来/看边界/读 diff），见 [环节 05](../../phases/05-build/README.md) |
| 顺手重构 | 任务外"优化" = 范围蔓延 | 越界改动登记新任务卡，不顺手做 |
| 一天一 commit 都没有 | "大干三天再提交" | 主分支每天收工时必须可运行 |

## 本目录文件

| 文件 | 什么时候读 |
|---|---|
| [frontend.md](frontend.md) | 完整方法论：技术栈选择、测试金字塔、性能优化 |
| [multi-platform-build.md](multi-platform-build.md) | **要一次开发多端出包时**（iOS/Android/H5/小程序/Steam）：路线决策树 + 一条命令对照 + CI matrix |
| [README.md](README.md)（本页） | 交接流程与契约 |

## 带走清单（独立使用本目录时）

- `phases/05-build/SKILL.md` —— 任务卡执行技能（每张卡的施工规范）
- `docs/03-ui/` 的 `pages.md`/`design-tokens.md` 产出物要求 —— 你的输入格式
- `skills/backend/api-design.md` —— 上游接口契约的格式（要对得上）
