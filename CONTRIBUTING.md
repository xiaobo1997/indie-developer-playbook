# 贡献指南

感谢你想完善这份独立开发者手册！这是一个「边做产品边记笔记」的活仓库，欢迎任何形式的贡献。

## 贡献方式

### 1. 修正与补充内容（最简单）
- 修正错别字、失效链接、过时的平台规则
- 直接提 Pull Request，说明改了什么、为什么

### 2. 补充环节内容
- 每个环节（`phases/XX-xxx/`）固定三件套：`README.md` / `references.md` / `SKILL.md`
- 新增外部链接请遵守 [ADR-0004](docs/decisions/0004-external-references-policy.md)：只收验证过的链接 + 一句话说明
- 新增小程序踩坑经验：写进对应环节 README 的「小程序主线注意点」

### 3. 沉淀新技能（SKILL.md）
- 结构固定六段：任务目标 / 输入 / 输出 / 执行步骤 / 检查清单 / 边界
- 格式要求见 [ADR-0002](docs/decisions/0002-agent-agnostic-skills.md)：纯 Markdown，frontmatter 只写 name 和 description
- 好技能的标准：换个没上下文的 AI 代理执行，也能按「输出契约」交付合格结果

### 4. 结构性变更
- 涉及目录结构、技能格式等跨模块约定的修改，先开 Issue 或起草一条 [ADR](docs/decisions/) 讨论

## 提交规范

- 提交信息：`docs: 新增 XX 环节的变现参考` 这类「前缀 + 中文摘要」
- 实质修改必须同步更新 [CHANGELOG.md](CHANGELOG.md) 的 `[Unreleased]` 小节
- 一次 PR 聚焦一件事，方便 review

## 内容原则

1. **可执行 > 正确的废话**：给步骤、给模板、给链接，不给空洞口号
2. **链接不复制**：环节文档引用 docs/ 知识库时用链接，避免两处维护同一内容
3. **以官方为准**：平台规则、价格、限额等易变信息标注「以官方文档为准」
4. **诚实**：案例必须可公开验证；不确定的就说不确定

## 许可

提交即表示你同意内容以 [MIT License](LICENSE) 发布。
