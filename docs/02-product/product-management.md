# 02 - PM 产品设计

## 独立开发者的 PM 工作

作为独立开发者，PM 工作包括：
1. 找需求
2. 定义 MVP
3. 写 PRD
4. 优先级排序
5. 数据分析

## 找需求

### 来源
1. **自己**：自己的工作流、痛点
2. **社区**：Twitter、Reddit、即刻、V2EX
3. **竞品**：App Store 评论、Product Hunt
4. **趋势**：Google Trends、Indie Hackers

### 验证方法
- **5 人法则**：找 5 个潜在用户聊 30 分钟
- **Landing Page**：做一个单页 + 投放广告，看转化
- **Pre-order**：让用户预付定金
- **MVP**：30 天做出最简版本看是否有人用

### 为什么这一步最容易被跳过

101 场创始人访谈里，「没做用户验证就开建」被 **11 位创始人主动提及**，在所有失败模式中排第四。它也是唯一一个会被「程序员身份」系统性放大的模式——因为我们能在完全没有用户的情况下自娱自乐很久。

- **早期信号**：心里的想法是「等做好了再给人看」
- **不可挽回点**：连续 6 个月没有外部反馈

完整数据与其余 6 个失败模式见 [13-cases：101 场创始人访谈](../13-cases/case-studies.md#数据101-场创始人访谈)。

### 开源参考：想法评估

[solo-founder-playbook](https://github.com/yayashuxue/solo-founder-playbook)（★25）——6 个 Claude Code skills，全部建在 101 场创始人访谈的结构化数据上。与本环节直接相关的是两个：

- `solo-analyze`：给一个想法，输出结构化评估
- `solo-roast`：反过来，主动找这个想法最可能死在哪

star 数很低，但它的价值在于随仓库附带的 knowledge 目录（原始访谈的结构化数据），值得当案例库读。

## MVP 设计

### MVP 原则
- 解决**一个**核心问题
- 一个人 1-2 周能做出
- 可以快速迭代
- 不需要完美

### MVP 反模式
- "平台化"（做生态）
- "全功能"（所有用户都满足）
- "完美"（设计精美、无 bug）
- "等"（等技术成熟）

### MVP 模板
```
产品名：
目标用户：
核心问题：
解决方案：
关键功能 (1-3 个)：
不做的事：
变现方式：
成功指标：
```

## 写 PRD

PRD (Product Requirements Document) 是独立开发者的"草稿纸"。

### 模板
[参考 templates/prd-template.md](../../templates/prd-template.md)

### PRD 写作原则
- **3 页内**：超过就太复杂
- **图比字多**：画流程图、线框图
- **例子驱动**：举具体场景
- **明确不做**：非目标同样重要

## 优先级排序

### RICE 框架
- **Reach**：影响多少用户
- **Impact**：影响多大（1-3 分）
- **Confidence**：把握度（百分比）
- **Effort**：工作量（人天）

分数 = (Reach × Impact × Confidence) / Effort

### MoSCoW 法则
- **Must have**：必须有（不做就别上线）
- **Should have**：应该有（重要但非关键）
- **Could have**：可以有（有最好，没有也行）
- **Won't have**：不会有（这版不做）

## 数据分析

### 关键指标（海盗指标 AARRR）
- **Acquisition**：获取（新用户）
- **Activation**：激活（首次使用）
- **Retention**：留存（持续使用）
- **Revenue**：收入（付费）
- **Referral**：传播（推荐他人）

### 推荐工具
- **Plausible**：隐私友好的网站分析
- **PostHog**：产品分析（事件、漏斗）
- **Umami**：自托管分析
- **微信自带**：小程序数据分析

### 关注的核心问题
- 多少人用？（DAU/WAU/MAU）
- 多少人付钱？（转化率）
- 付多少钱？（ARPU/LTV）
- 留多久？（留存率）

## 工具栈

| 用途 | 工具 |
|---|---|
| 文档 | Notion / 飞书 / Obsidian |
| 任务 | Todoist / TickTick |
| 原型 | Figma / 即时设计 |
| 反馈 | Canny / Productboard |
| 分析 | Plausible / PostHog |
| 用户访谈 | Calendly + Zoom |

---

## 实战模板

参见 [skills/product](../../skills/product/)：
- 用户访谈模板
- PRD 模板
- 用户故事模板
- 优先级评估表

