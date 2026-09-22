# UI 角色的参考项目库

> **本页是 UI 角色（`docs/03-ui`）独立跑完环节 03 所需的参考。** 收录政策见 [ADR-0004](../decisions/0004-external-references-policy.md)：所有仓库经 GitHub API 逐个核实（存在 / 未归档 / 有 License / 最后更新时间），star 数与更新时间截至 **2026-09-20**。
>
> 为什么收 GitHub 仓库而不是设计网站：**网站会打不开，仓库不会**。Mobbin、Plausible 官网这类站点在部分网络下访问不稳定，而仓库地址长期可用——这也是 ADR-0004 优先收录仓库的原因。

## UI 角色独立跑完环节 03 的流程

1. **先想清楚要什么** → [prototype-first.md](prototype-first.md)：核心功能守门 + 页面清单。**没做这一步别来看参考**，否则你只会抄回一堆用不上的页面

2. **认出自己需要哪几种页面骨架** → 见下一节
3. **按骨架找同类项目**（本页 A-D 组），**不要通读，只看你要的那一屏**
4. **画成线框** → [wireframe-preview.html](../../templates/wireframe-preview.html)，按手机真实比例看，切换空/载/错三态
5. **选组件库开写** → 小程序看 D 组，Web 看 [ui-design.md](ui-design.md) 的设计系统一节

## 先看这个：独立应用的 6 种页面骨架

「不知道独立应用的 UI 长什么样」真正的答案不是多看几个项目，而是认出**界面就这 6 种**。你的产品大概率是它们的组合——先认出自己在用哪几种，再去看对应的项目。

| # | 骨架 | 长什么样 | 新手最容易做错 |
|---|---|---|---|
| 1 | **Dashboard** | 顶部 2-4 个 KPI 数字卡 → 下方图表 / 列表 | 卡片太多，一屏超过 4 个数字就没有焦点了 |
| 2 | **列表 + 详情** | 搜索 + 筛选 → 条目列表 → 点进详情（移动端是列表页点进详情页） | 只做「有数据」的样子，忘了空态、加载、分页 |
| 3 | **表单** | 分组标题 → 单列输入项（标签在上）→ 底部固定主按钮 | 字段堆一屏，应该拆成 2-3 步 |
| 4 | **设置** | 分组导航（账号 / 偏好 / 计费）→ 每行一个设置项（标签 + 控件） | 把设置当功能页做，塞进一堆操作按钮 |
| 5 | **落地页** | 一句话价值主张 → 产品截图 → 3 个特性 → 定价表 → CTA | 讲功能不讲结果（用户不关心你用了什么技术） |
| 6 | **空态** | 一个图标 + 一句话 + 一个按钮 | 留白，或只写「暂无数据」——这是新用户的第一屏 |

**去看哪个项目**：Dashboard 看 plausible（最克制）/ twenty（最现代）；列表详情看 outline / twenty；表单看 formbricks / documenso；设置看 cal.diy（分组最全）/ plausible（最简）；落地页看 plausible 首页；空态看任何项目的空列表状态。

## 必须先知道的三件事（核实结果）

| 常见引用的地址 | 实际情况 |
|---|---|
| `calcom/cal.com` | **已改名 `calcom/cal.diy`**（★48,567，MIT）。旧链接会跳转，但别再存旧名 |
| `wechat-miniprogram/colorui-beta` | **404，不存在**。真实仓库是 **`weilanwl/coloruicss`**（★12,377，MIT），且**最后更新 2024-04-08，已停更两年** |
| 多个项目是 AGPL / GPL | plausible、AppFlowy、documenso 是 **AGPL-3.0**，AnkiDroid 是 **GPL-3.0**。**看界面布局没问题，但复制代码进商业产品有传染性风险** |

---

## A. 极简 Dashboard 与卡片布局

| 仓库 | star | 许可证 | 学什么 |
|---|---|---|---|
| [plausible/analytics](https://github.com/plausible/analytics) | 29,157 | AGPL-3.0 | **极简产品教科书**。一个开发者维护，主页 / dashboard / 设置全是卡片化，配色克制。学 Dashboard 卡片布局、设置页极简化、首页营销页设计 |
| [calcom/cal.diy](https://github.com/calcom/cal.diy) | 48,567 | MIT | **完整 SaaS UI 范本**（原名 cal.com）。注册 → 预订 → 支付完整流程，移动端响应式，文档超完整（学它的 docs/）。学注册流、定价页、设置页组织 |
| [dubinc/dub](https://github.com/dubinc/dub) | 24,782 | 自定义 | 卡片式 dashboard + 极简现代感 + 数据可视化 |
| [outline/outline](https://github.com/outline/outline) | 40,632 | 自定义 | 开源 Notion 替代。文档类 UI 标准：卡片 + 编辑器 + 多视图切换（列表 / 看板） |
| [AppFlowy-IO/AppFlowy](https://github.com/AppFlowy-IO/AppFlowy) | 76,854 | AGPL-3.0 | 中文友好，开源 Notion 替代，一整套块级 UI 设计 |

## B. 学习 / 记忆类应用的界面

| 仓库 | star | 许可证 | 学什么 |
|---|---|---|---|
| [ankidroid/Anki-Android](https://github.com/ankidroid/Anki-Android) | 11,823 | GPL-3.0 | 复习卡片的标准 UI，SM-2 算法实现参考。极简但功能完整——**直接学它的复习页设计** |
| [ankitects/anki](https://github.com/ankitects/anki) | 31,334 | 自定义 | 卡片正面 / 反面设计、评分按钮布局、难度系统 UI |

## C. 开源 SaaS 设计精品

| 仓库 | star | 许可证 | 学什么 |
|---|---|---|---|
| [twentyhq/twenty](https://github.com/twentyhq/twenty) | 57,119 | 自定义 | 开源 CRM，**极简 + 卡片的代表**。多视角切换、数据表卡片化。**和「独立应用该长什么样」这个问题高度匹配，建议第一个看** |
| [formbricks/formbricks](https://github.com/formbricks/formbricks) | 12,966 | 自定义 | 开源 Typeform。表单 + 卡片设计、进度可视化、主题定制 |
| [documenso/documenso](https://github.com/documenso/documenso) | 15,107 | AGPL-3.0 | 开源电子签名。完整的发票 / 合同流程 UI、卡片化 dashboard、响应式 |

## D. 微信小程序（本仓库主线，优先看这组）

| 仓库 | star | 许可证 | 学什么 |
|---|---|---|---|
| [Tencent/tdesign-miniprogram](https://github.com/Tencent/tdesign-miniprogram) | 1,764 | MIT | **腾讯官方小程序组件库**，可直接 import 用。选定后别换（环节 03：一个项目只用一个库） |
| [wechat-miniprogram/miniprogram-demo](https://github.com/wechat-miniprogram/miniprogram-demo) | 7,234 | MIT | 官方示例：组件 / API / 云开发的用法，完整项目结构 |
| [weilanwl/coloruicss](https://github.com/weilanwl/coloruicss) | 12,377 | MIT | 高饱和色彩、专注视觉的 CSS 组件库。⚠️ **最后更新 2024-04-08，已停更两年**——可参考配色思路，别在新项目里依赖它 |

---

## E. AI 生成 UI 的工具（**都不支持小程序**，做小程序直接看 D 组）

> ⚠️ **先说清楚**：下面这些工具生成的是 **Web（React / Tailwind / HTML）**，**不能用于微信小程序**。小程序用 WXML / WXSS，与 Web 组件不通用。
> **你要做小程序，请直接跳回上面的 D 组。**
>
> 这些工具的真正价值：**在写代码之前先看到「这个想法做出来长什么样」**——用来判断方向，不是用来交付。

可访问性实测（2026-09-21，HTTP 状态码）：

| 工具 | 状态 | 干什么 | 什么时候用 |
|---|---|---|---|
| [v0.dev](https://v0.dev) | 200 | 文本 → React / Tailwind 组件代码 | **首选**。产出是可改的代码，不是图 |
| [Bolt.new](https://bolt.new) | 200 | 文本 → 完整可跑的全栈应用 | 想看到能点、能用的东西时 |
| [Lovable](https://lovable.dev) | 403\* | 文本 → Web 项目 + 后端 | MVP demo |
| [Uizard](https://uizard.io) | 200 | 截图 / 草图 → UI 设计稿 | 已有草图，想变成设计稿 |
| [Visily](https://visily.ai) | 200 | 同上，截图转 UI 更准 | 草图转高保真 |
| [Motiff](https://www.motiff.com) | 200 | 国内 AI 设计工具，Figma 平替 | 中文场景、不想用英文界面 |

\* 403 是站点反爬，不代表不可用。**实测 000 的才是真的连不上。**

| 流程图 / 架构图（不是 UI，是流程） | 状态 |
|---|---|
| [Whimsical](https://whimsical.com)（文本 → 流程图 / 线框 / 思维导图） | 200 |
| [Eraser](https://eraser.io)（文本 → 架构图 / 流程图 / ER 图） | 200 |
| [tldraw](https://tldraw.com)（手绘风格 + AI 整理） | 200 |

### 一个已经不该出现在工具链里的

| 工具 | 情况 |
|---|---|
| Galileo AI | 官网 `galileoai.com` 实测 **000（连不上）**。它被标注为「已被 Canva 收购」，而独立产品已经不可访问——**别再把它放进你的工具链** |

这个例子正好说明本仓库为什么优先收 GitHub 仓库而不是 SaaS 工具：**工具会被收购、关停、改价，仓库地址不会。**

### 关于图像生成模型做 UI 概念图

GPT-Image / Midjourney / Flux / Ideogram / Recraft 这类模型能出 UI 概念图，但**版本号与能力迭代极快（半年就过时）**，本仓库不收具体版本。

要提醒的是：它们产出的是**概念图，不是可开发的东西**——没有真实数据结构、没有空态、组件是你组件库里没有的。这正是 [ai-as-designer.md](ai-as-designer.md) 里说的 **Dribbble 页面**。看方向可以，别照着实现。


## 许可证速查

**看界面、学布局、抄结构都不涉及许可证。只有复制代码进你的产品才有约束。**

| 类别 | 仓库 | 能不能抄代码进商业产品 |
|---|---|---|
| MIT（最宽松） | cal.diy、tdesign-miniprogram、miniprogram-demo、coloruicss | ✅ 可以，保留版权声明即可 |
| AGPL-3.0 | plausible、AppFlowy、documenso | ⚠️ 有传染性：改了用在在线服务里，要求你也开源 |
| GPL-3.0 | AnkiDroid | ⚠️ 同上（比 AGPL 略宽松，但仍不适用于闭源产品） |
| 自定义（GitHub 未识别） | twenty、formbricks、outline、dub、anki | ❓ 需自行查看仓库根目录 LICENSE 文件确认 |

## 怎么最快看到界面

光有仓库链接还不够——你要的是「看见」。三个办法，按成本排序：

1. **仓库 README 首屏截图**：最快，30 秒就能判断这个产品的界面风格是不是你想要的
2. **官方在线 demo**：多数开源 SaaS 项目 README 里会放 demo 地址（如果打不开，回到仓库看截图）
3. **本地跑一遍**：`git clone` + 按 README 的 Quick Start 起服务。最慢但最真实，值得为 1-2 个你最想抄的项目做一次

**别试图通读这 13 个项目。** 认出你要的页面骨架，去对应的那一个项目里看那一屏，然后用线框工具画出来——这是环节 03 的全部工作量。

## 相关

- [prototype-first.md](prototype-first.md)：核心功能守门 → 页面清单 → 可预览线框 → 组件库认领
- [ui-design.md](ui-design.md)：设计系统、工具栈、组件库
- [环节 03 UI/UX 设计](../../phases/03-design/README.md)
- [templates/wireframe-preview.html](../../templates/wireframe-preview.html)：线框预览工具
