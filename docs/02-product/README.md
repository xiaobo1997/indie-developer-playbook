# 02 · PM 产品角色目录

> PM 是独立开发者的第一个角色，也是最容易跳过的角色。本目录让你独立跑完"从想法到可开工的 PRD"，不依赖任何其他角色先就位。
> 对应环节：[环节 01 验证](../../phases/01-validate/README.md) + [环节 02 定义](../../phases/02-spec/README.md)。

## 这个角色负责什么

| 负责 | 不负责 |
|---|---|
| 找需求、验证真假、下做/不做的结论 | 写代码实现（前端/后端角色） |
| 写 PRD、定 MVP 范围、排优先级 | 画页面长什么样（UI 角色） |
| 定义成功指标、事后用数据复判 | 决定技术栈（脚手架环节的事） |

**边界判定**：产出物是"决策与文档"（验证报告、PRD、任务卡）；产出物是"界面或代码"就越界了。

## 独立跑完「PM 工作」的流程

| 步 | 做什么 | 产出 | 耗时 |
|---|---|---|---|
| 1 | 一句话假设 + 竞品扫描 + 5 人访谈 | 验证报告 | 3 天 |
| 2 | 做/不做/调整 三选一结论 | 一行决定 + 理由 | 半天 |
| 3 | 按模板写一页 PRD（非目标必填） | `prd.md` | 半天 |
| 4 | MVP 功能过砍掉测试，排 P0-P2 | 功能范围表 | 1 小时 |
| 5 | 任务卡拆到 ≤1 天 + 3 个里程碑 | `tasks.md` | 半天 |

数据警示（[101 场访谈](../13-cases/case-studies.md#数据101-场创始人访谈)）："没做用户验证就开建"被提及 11 次、排失败模式第 4——它也是被程序员身份系统性放大的一条。步 1 不可跳。

## 收什么 → 交什么

| 上游交给我 | 我交给下游 |
|---|---|
| 想法、自己的痛点 | → UI 角色：一页 PRD + 页面结构粗稿 |
| 用户反馈与数据（来自客服/复盘） | → 全体角色：`tasks.md` 任务卡（含完成标准） |
| | → 营销角色：一句话定位 + 目标用户画像 |

## 真实参考（不用凭空想象）

- 模板：[templates/prd-template.md](../../templates/prd-template.md)（11 节结构现成）
- 方法书：[The Mom Test](https://www.momtestbook.com/)（问行为不问观点）、[Shape Up](https://basecamp.com/shapeup)（先定时间预算再定范围）
- 想法评估开源工具：[yayashuxue/solo-founder-playbook](https://github.com/yayashuxue/solo-founder-playbook)（★26）的 `solo-analyze` / `solo-roast`——给想法做结构化体检 / 主动找死因
- 本仓库技能：[skills/product/](../../skills/README.md)（prd-template ✅ / user-story ✅ / user-interview ✅）
- 优先级框架：RICE 与 MoSCoW 速查见 [product-management.md](product-management.md)

## 新手最容易踩的坑

| 坑 | 为什么会踩 | 怎么避 |
|---|---|---|
| 跳过验证直接开发 | "我先做出来再说" | 访谈数据 11 次提及；验证 3 天，返工 3 个月 |
| PRD 写 50 页 | 把写文档当进展 | 一页纪律；细节靠任务卡按需补 |
| MVP 不最小 | "反正很快" | 每个功能过砍掉测试：砍掉会丢用户吗？ |
| 给朋友发问卷 | 客套话污染数据 | 访谈问过去行为；问卷只做行为验证的辅助 |

## 本目录文件

| 文件 | 什么时候读 |
|---|---|
| [product-management.md](product-management.md) | 完整方法论：找需求、MVP、RICE/MoSCoW、AARRR |
| [README.md](README.md)（本页） | 独立跑完 PM 工作的流程与契约 |

## 带走清单（独立使用本目录时）

- `templates/prd-template.md` —— 流程步 3 的模板
- `phases/01-validate/` 与 `phases/02-spec/` 的 SKILL.md —— 步 1 与步 3-5 交给 AI 执行的技能
- `docs/13-cases/case-studies.md#数据101-场创始人访谈` —— 数据依据
