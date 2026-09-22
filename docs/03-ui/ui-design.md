# 03 - UI/UX 设计

> 想先看「怎么从一个想法走到可预览的页面」，读 [prototype-first.md](prototype-first.md)：核心功能守门 → 页面清单 → 可预览线框 → 组件库认领，四步。
> 想知道「独立应用的 UI 到底长什么样」，读 [ui-reference-projects.md](ui-reference-projects.md)：6 种页面骨架 + 13 个经核实的开源参考项目（含 3 处链接纠正与许可证速查）。
> 本页讲更细的设计系统与工具栈。

## 独立开发者的设计挑战

### 痛点
- 不是设计师出身
- 没时间深入学习
- 工具复杂

### 解决方案
- **以工程思维做设计**：组件化、复用
- **AI 辅助**：用 v0、Figma AI 生成
- **模仿成熟产品**：参考 Notion、Linear 等设计

## 设计原则

### 1. 内容优先 (Content First)
- 文字比装饰重要
- 留白比满版重要
- 可读性比美观重要

### 2. 一致性 (Consistency)
- 颜色：3-5 个主色
- 字体：1-2 种
- 间距：4 / 8 / 16 / 24 / 32 / 48
- 圆角：4 / 8 / 12 / 16

### 3. 反馈 (Feedback)
- 每个操作都有反馈（按钮变色、loading）
- 错误有提示（不要只 console.log）
- 成功有庆祝（toast、动效）

### 4. 渐进式 (Progressive)
- 移动端优先
- 暗色模式
- 无障碍（A11y）

## 设计系统

### 必备元素
```
颜色：
  主色 (Primary):    #6366f1 (蓝紫)
  辅助 (Secondary):  #8b5cf6
  成功 (Success):    #10b981
  警告 (Warning):    #f59e0b
  错误 (Danger):     #ef4444
  文字 (Text):       #1f2937 / #6b7280 / #9ca3af
  背景 (Bg):         #ffffff / #f9fafb / #f3f4f6
  
字号：
  xs:   12px
  sm:   14px
  base: 16px
  lg:   18px
  xl:   20px
  2xl:  24px
  3xl:  30px
  
间距：
  4, 8, 12, 16, 24, 32, 48, 64
  
圆角：
  sm: 4px
  md: 8px
  lg: 12px
  xl: 16px
  full: 9999px
  
阴影：
  sm: 0 1px 2px rgba(0,0,0,0.05)
  md: 0 4px 6px rgba(0,0,0,0.07)
  lg: 0 10px 15px rgba(0,0,0,0.1)
```

## 工具栈

### 设计工具
| 工具 | 价格 | 适合 |
|---|---|---|
| **Figma** | 免费起步 | 全平台，协作强 |
| **即时设计** | 免费 | 中文，国产替代 |
| **Sketch** | $9/月 | 仅 macOS |
| **Penpot** | 免费 | 开源 |

### AI 设计工具
| 工具 | 用途 |
|---|---|
| **v0.dev** | 文字生成 shadcn UI 代码 |
| **Figma AI** | 设计稿生成 |
| **Galileo AI** | UX 探索 |
| **Recraft** | AI 图像 |

### 设计资产
- **shadcn/ui** - 现代 UI 组件（强烈推荐）
- **heroicons** - 免费 SVG 图标
- **unDraw** - 免费插画
- **Unsplash** - 免费图片
- **Google Fonts** - 免费字体
- **iconify** - 海量图标库

## 工作流

### 1. 先草图 (Sketch)
- 用纸笔画线框图
- 不要追求好看，追求想法

### 2. 再低保真 (Wireframe)
- Figma 画灰度线框图
- 关注布局和流程

### 3. 再高保真 (Mockup)
- 上色、加细节
- 准备交付

### 4. 实现 (Code)
- 设计师直接写代码（独立开发者优势）
- 用 Tailwind CSS 加速

## 移动端设计要点

### 微信小程序
- 设计稿用 750×1334px (iPhone 6)
- 一屏只显示一个核心操作
- 避免左右滑动手势（微信不支持）
- 字号 ≥ 24rpx (12pt)

### iOS App
- 设计稿用 393×852px (iPhone 15 Pro)
- 遵循 iOS Human Interface Guidelines
- 状态栏 44pt，导航栏 44pt
- 底部安全区 34pt

### Android
- 设计稿用 360×800px (mdpi)
- 遵循 Material Design 3
- 状态栏 24dp，导航栏 56dp

## 设计原则：参考

### 极简风
- Notion, Linear, Things 3
- 特点：留白、灰度、少装饰

### 友好风
- Figma, Pitch
- 特点：渐变色、插画、动效

### 工具风
- VSCode, GitHub
- 特点：信息密度高、紧凑

## 设计系统模板

独立开发者推荐：

### shadcn/ui
- 基于 Radix UI + Tailwind
- 复制粘贴组件（不是 npm 包）
- 高度可定制
- 现代美观

### Tailwind UI
- Tailwind 官方组件库
- 付费，但质量高

### Ant Design / Arco Design
- 国内大厂出品
- 完整、适合后台系统

## 推荐学习

### YouTube
- **Design Course**：设计基础
- **The Futur**：设计商业
- **Jesse Showalter**：Figma 教程

### 必读书
- 《Refactoring UI》- 工程师友好
- 《Don't Make Me Think》- UX 入门
- 《Design of Everyday Things》- 设计思维

