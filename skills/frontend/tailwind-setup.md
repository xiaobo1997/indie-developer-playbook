---
name: tailwind-setup
description: Tailwind 配置技能：为 Web 项目生成与 design-tokens 对齐的 Tailwind 配置（色板/字阶/间距/圆角），并验证 tokens 生效。不适用于微信小程序。
---

# 任务目标

接环节 03 产出的 `design-tokens.md`（或一页设计约定），为 Web 项目生成**可直接使用的 Tailwind 配置**，保证全局样式与设计约定一致。⚠️ **仅适用于 Web 路线（React/Vue/Next）；小程序是 WXSS，本技能不适用。**

# 输入

1. `design-tokens.md`（主色 / 语义色 / 字号阶梯 / 间距 / 圆角）；没有就先向用户要，或按 docs/03-ui/ui-design.md 的默认值起草并标注"草案"
2. 技术栈与 Tailwind 版本（v3 配置文件 / v4 CSS-first，不确定就问一轮）

# 输出（严格按此结构交付）

```
## 配置代码（完整可复制）
v3: tailwind.config.js 完整内容（theme.extend 内全部 token）
v4: @theme CSS 块完整内容

## 语义色映射
success / warning / danger / 文字三级 / 背景三级 → 具体变量名与色值

## 验证步骤
1-2-3：起 dev server → 在任意页面加验证类名 → 应看到什么

## 与组件库的冲突检查
若使用 shadcn/ui：说明 CSS 变量如何对接（不覆盖其默认变量，只做主题层）
```

# 执行步骤

1. 先读 [docs/03-ui/ui-design.md](../../docs/03-ui/ui-design.md) 的设计系统一节与 [docs/03-ui/prototype-first.md](../../docs/03-ui/prototype-first.md) 的「只做三个决定」——tokens 里没有的值一律用 Tailwind 默认，不新增
2. 按 design-tokens 生成配置：只扩展 tokens 里有的项；禁止顺手加自定义阴影/动画/字体
3. 语义色用 CSS 变量挂接（方便未来深色模式），给出变量命名规范
4. 与所用组件库对接：shadcn/ui 项目不改它的变量名，只改值
5. 给最小验证步骤，让用户 5 分钟内确认生效

# 检查清单（输出前自检）

- [ ] 配置完整可复制，没有 "...其余省略"
- [ ] 没有引入 design-tokens 之外的新值；默认值未被无谓覆盖
- [ ] 标注了 Tailwind 版本差异（v3/v4 写法不同）
- [ ] 页面顶部明确"不适用于小程序"

# 边界（不要做什么）

- 不搭整个项目骨架（那是 `phases/04-setup/SKILL.md`）
- 不定设计决策（颜色/圆角有分歧时回环节 03，不在配置里即兴创作）
- 不适用：小程序项目（用组件库 + WXSS）；不想用 Tailwind 的项目（直接用组件库默认样式即可，别为用而用）
