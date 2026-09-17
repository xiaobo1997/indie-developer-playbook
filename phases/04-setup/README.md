# 环节 04 · 技术选型与脚手架（Setup）

> **解决什么问题**：用最少的决定把"能跑的空项目"搭出来。单人开发选型只有一个标准：**优先选 AI 最熟、社区最大、你要付的额外决策最少的那条路**，炫技架构是负债。

## 你要做什么

1. **前端路线（一个决定）**：
   - **原生小程序**：依赖最少、和官方文档一一对应、AI 都熟悉——**你的第一个小程序建议选它**
   - **Taro / uni-app**：将来确定要多端（同时出 H5/App）再上，代价是多一层编译抽象
   - 判断法：除非你今天就要"一套代码多端发布"，否则选原生
2. **后端路线（一个决定）**：
   - **微信云开发**：官方托管（数据库/存储/云函数），免域名免备案免运维——**个人开发者默认选它**
   - **自建（Node/Go + 服务器）**：需要 HTTPS + ICP 备案 + 小程序后台配合法域名；你有服务器运维经验且怕锁定再选
   - **BaaS（Supabase / PocketBase）**：适合 Web 路线；小程序配合自建域名用
3. **环境清单（半小时）**：微信开发者工具（稳定版）、Node LTS、Git；小程序 AppID 注册好，`project.config.json` 里的 appid 填上（测试号也先跑通再说）。
4. **脚手架与仓库（1 小时）**：`git init` + 目录按[官方推荐结构](https://developers.weixin.qq.com/miniprogram/dev/framework/structure.html)来；放进三份文件：`AGENTS.md`（抄本仓库根目录改写成项目版）、`CLAUDE.md` 或同等说明（部分工具认这个名，内容可相同）、`README.md`（怎么跑起来）。
5. **跑通 Hello World 闭环**：模拟器能跑 + **真机预览能跑** + 提交第一个 commit。没跑通闭环之前不进入环节 05。

## 产出物

- 可运行、真机可预览的空项目仓库
- 项目版 `AGENTS.md`（技术栈、目录、代码约定、常用命令）
- 一页 `DECISIONS.md`（或直接在项目仓库建 docs/decisions/）：记录你选了什么、为什么

## 完成标准

- [ ] 模拟器和真机都能跑通空项目
- [ ] `AGENTS.md` 就位：AI 拿到项目就知道目录结构与约定
- [ ] 后端方案已定，且云环境/服务器已开通可用
- [ ] Git 仓库有第一个 commit，README 写了"三行跑起来"

## 常见陷阱

- **选型纠结超过半天**：这是决策不是研究。记住默认答案：原生小程序 + 云开发；不满再来改（改造成本比想象中低，因为第一版本来就小）。
- **第一天就配 CI/CD、ESLint 全家桶**：先让产品跑起来。单人项目，Git + 自律足够撑到 1000 用户。
- **不用版本控制**："先在本地随便写写"——请从第一行代码就 commit，这是你最便宜的时间机器。
- **把 appid 提交进公开仓库**：appid 本身泄露风险不大，但密钥（AppSecret）绝不能进仓库；`.gitignore` 从第一个 commit 就配好。

## 小程序主线注意点

- **主体注册要趁早**：个人主体注册免费、当天可用；企业主体需要营业执照。AppID 是一切的前提，本环节第一件事就去[注册](https://mp.weixin.qq.com/)。
- **开发者工具选「稳定版」**： Nightly/RC 版有坑；工具内"详情 → 本地设置"里勾选项（如 ES6 转 ES5、校验合法域名）影响真机表现，团队默认关掉"校验合法域名"只用于本地调试，提审前记得恢复。
- **云开发有免费额度**：个人项目初期基本免费，注意用量监控（以[官方文档](https://developers.weixin.qq.com/miniprogram/dev/wxcloud/)为准），避免某个死循环云函数烧光额度。
- **未来要 App？**：先小程序验证，别为"以后可能要 App"提前上 Taro/uni-app——YAGNI。

## 深入学习（角色知识库）

- [docs/10-platforms/wechat/miniprogram.md](../../docs/10-platforms/wechat/miniprogram.md)：小程序平台完整指南 ✅
- [docs/04-frontend](../../docs/04-frontend/frontend.md) / [docs/05-backend](../../docs/05-backend/backend.md)：前后端角色详解
- [docs/12-tools/stack.md](../../docs/12-tools/stack.md)：推荐工具栈
- [AGENTS.md](../../AGENTS.md)：给 AI 的项目说明怎么写（照抄改造）

## 本环节文件

- [references.md](references.md)：官方文档与脚手架的外部链接
- [SKILL.md](SKILL.md)：把选型与脚手架交给 AI 的标准技能
