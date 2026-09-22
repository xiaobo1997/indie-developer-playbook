# 06 · DevOps 角色目录

> DevOps 角色管"能不能稳定地被用户用到"：部署、监控、备份、回滚。单人项目原则：**PaaS 优先、自动化一切、简单可靠 > 高级**——不学 Kubernetes，不过早优化。
> 对应环节：[环节 06 部署与上线](../../phases/06-release/README.md) + [环节 08](../../phases/08-iterate/README.md) 的监控部分。

## 这个角色负责什么

| 负责 | 不负责 |
|---|---|
| 部署、域名、HTTPS、发版回滚 | 写业务代码（前端/后端角色） |
| 监控告警、错误追踪 | 决定功能优先级（看数据给 PM 提供） |
| 备份策略与恢复演练、成本控制 | 安全规则制定（与后端角色共担，清单见 skills） |

**边界判定**：产出物是"在线的版本 + 可观测性 + 可恢复性"；产出物是"接口"或"页面"就越界了。

## 独立跑完「DevOps 工作」的流程

| 步 | 做什么 | 产出 | 耗时 |
|---|---|---|---|
| 1 | 选部署平台（小程序：微信后台；Web：Vercel/CF Pages） | 一行选型 + 理由 | 半天 |
| 2 | 首次部署 + 域名 + HTTPS（自建需 ICP 备案） | 可访问的线上版本 | 1 天 |
| 3 | 监控就位：错误上报 + 在线拨测 + 报警触达 | 能收到报警的渠道 | 半天 |
| 4 | 备份：按 3-2-1 生成方案 + 做一次恢复演练 | 备份矩阵 + 演练记录 | 半天 |
| 5 | 发版流程固化：走一遍部署检查清单 | `release-checklist.md` | 每次发版 30 分钟 |

**平台速查**：Vercel（Next.js 首选）/ Cloudflare Pages（静态 + Workers）/ 微信 MP 后台（小程序发版与[运维中心](https://mp.weixin.qq.com/)看错误）。自建服务器完整指南见 [../10-platforms/self-hosted/self-hosted.md](../10-platforms/self-hosted/self-hosted.md)。

## 收什么 → 交什么

| 上游交给我（后端/前端角色） | 我交给下游 |
|---|---|
| 可运行的版本（主分支可跑） | → 全体：在线版本 + 线上环境变量清单 |
| 环境变量与迁移脚本 | → 营销角色：稳定的发布地址 |
| 数据资产清单（哪些要备份） | → PM/复盘：错误率与性能数据（进环节 08） |

## 真实参考（不用凭空想象）

- 部署：[Vercel](https://vercel.com/home)、[Cloudflare Pages](https://pages.cloudflare.com/)；小程序发版见 [../10-platforms/wechat/miniprogram.md](../10-platforms/wechat/miniprogram.md)
- 拨测监控：[Uptime Kuma](https://github.com/louislam/uptime-kuma)（一人写的开源监控，自托管）；错误追踪 Sentry（免费额度）
- 备份与恢复参照：[PocketBase](https://github.com/pocketbase/pocketbase) 的单文件快照模型
- 开源替代清单：[awesome-solo-founder-oss](https://github.com/princepal9120/awesome-solo-founder-oss)（★66）的部署类目
- 本仓库技能：[deploy-checklist](../../skills/devops/deploy-checklist.md) ✅ / [incident-response](../../skills/devops/incident-response.md) ✅ / [backup-strategy](../../skills/devops/backup-strategy.md) ✅

## 新手最容易踩的坑

| 坑 | 为什么会踩 | 怎么避 |
|---|---|---|
| 无监控上线 | "上线就算完成" | 步 3 是上线定义的一部分，不是加分项 |
| 备份从未验证恢复 | 备份脚本跑了就安心 | 每季度 30 分钟恢复演练（`backup-strategy` 内置清单） |
| 出事先 debug 不止血 | 工程师本能 | 铁律：回滚优先于 debug（`incident-response` 分级处置卡） |
| 第一天就上 K8s/CI 全家桶 | 简历驱动开发 | PaaS + Git 打 tag 足撑到 1000 用户 |
| 调试开关带上生产 | 本地调试改了配置忘恢复 | 部署检查清单的"调试遗留"组 |

## 本目录文件

| 文件 | 什么时候读 |
|---|---|
| [devops.md](devops.md) | 完整方法论：平台分级、CI/CD、监控、3-2-1 备份、成本控制 |
| [README.md](README.md)（本页） | 流程与契约 |

## 带走清单（独立使用本目录时）

- `skills/devops/` 三个技能（deploy-checklist / incident-response / backup-strategy）
- `phases/06-release/SKILL.md` —— 上线前审查技能（与 deploy-checklist 互补）
- 自建路线另带：`docs/10-platforms/self-hosted/self-hosted.md`
