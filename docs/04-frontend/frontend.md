# 04 - 前端开发

## 独立开发者的前端选择

### 推荐栈（按使用人数）

1. **React + Next.js** - 最主流，就业+生态最强
2. **Vue + Nuxt** - 国内友好，中文文档
3. **Svelte/SvelteKit** - 简洁，性能好
4. **Solid.js** - React-like，性能更好
5. **Astro** - 内容站首选

### 选型原则

| 场景 | 推荐 |
|---|---|
| 做 SaaS Web | Next.js / Nuxt |
| 做小程序 | Taro / uni-app |
| 做 App | React Native / Flutter |
| 做静态站 | Astro / Next.js (SSG) |
| 做工具 | Vite + React/Vue |

---

## 必备技能

### 基础（必学）
- HTML5 语义化
- CSS3（Flexbox + Grid）
- JavaScript ES6+（async/await、解构、模块）
- 响应式设计
- 浏览器开发者工具

### 进阶（强烈推荐）
- TypeScript
- 一个框架（React 或 Vue）
- 状态管理（Zustand/Pinia）
- CSS 框架（Tailwind CSS）
- 构建工具（Vite/Webpack）

### 高级（按需）
- 测试（Vitest/Playwright）
- SSR/SSG（Next.js/Nuxt）
- PWA
- Web Components
- 性能优化

---

## 推荐技术栈组合

### 组合 A：现代 SaaS
```
React 18 + TypeScript + Vite + Tailwind + shadcn/ui + Zustand
```
- 适合：Web SaaS
- 优势：生态最强、AI 友好

### 组合 B：Vue 全栈
```
Vue 3 + TypeScript + Vite + Element Plus + Pinia
```
- 适合：国内项目、中后台
- 优势：中文友好、模板多

### 组合 C：小程序 + App
```
Taro + React + TypeScript + NutUI
```
- 适合：小程序 + 未来 App
- 优势：多端输出

---

## AI 时代的开发流

### 工作流变化
```
传统：
需求 → 设计 → 编码 → 测试 → 部署

AI 辅助：
需求 → AI 辅助设计 → AI 生成代码 → 人工测试 → AI 辅助部署
```

### 推荐 AI 工具
| 工具 | 用途 |
|---|---|
| **Cursor** | AI 编辑器（强烈推荐）|
| **Claude Code** | 终端 AI 代理 |
| **v0.dev** | UI 代码生成 |
| **Bolt.new** | 全栈 AI 生成 |
| **Phrase** | i18n 翻译 |
| **Mage** | 图像生成 |

### Cursor 使用技巧
1. **Cmd + K**：内联编辑
2. **Cmd + L**：聊天
3. **Composer**：跨文件编辑
4. **@Docs**：引用文档
5. **Rules**：项目级 AI 规则

---

## 性能优化

### Core Web Vitals
- **LCP** (Largest Contentful Paint) < 2.5s
- **FID** (First Input Delay) < 100ms
- **CLS** (Cumulative Layout Shift) < 0.1

### 优化清单
- [ ] 图片懒加载、CDN
- [ ] 代码分割 (Code Splitting)
- [ ] Tree Shaking
- [ ] 字体子集化
- [ ] 关键 CSS 内联
- [ ] 预连接 (preconnect)

### 工具
- **Lighthouse** - 综合评分
- **PageSpeed Insights** - Google 工具
- **WebPageTest** - 详细报告
- **Bundlephobia** - 检查包大小

---

## 测试

### 测试金字塔
```
   E2E (10%)      - Playwright / Cypress
   ─────
  Integration (30%) - Vitest + Testing Library
  ─────
 Unit (60%)        - Vitest / Jest
```

### 必测的
- 核心业务逻辑（纯函数）
- 数据转换
- API 调用（mock）

### 不必测的
- UI 像素
- 第三方组件
- 简单 getter/setter

---

## 推荐学习资源

### 必读书
- 《JavaScript 高级程序设计》
- 《你不知道的 JavaScript》
- 《React 设计原理》
- 《TypeScript 编程》

### 必看文档
- MDN Web Docs
- React 官方文档（新版）
- Vue 3 官方文档

### 必订阅
- JavaScript Weekly
- React Newsletter
- Frontend Focus

### 视频
- Fireship（YouTube）- 短而精
- Theo - t3.gg
- Web Dev Simplified

