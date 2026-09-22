# 05 · 后端角色目录

> 后端角色管数据、接口与安全。单人项目里它的最高原则是**少建**：能托管就不自建，能单表就不分表，能 HTTP 就不 WebSocket。
> 对应环节：[环节 05 开发](../../phases/05-build/README.md)（后端部分）+ [环节 04](../../phases/04-setup/README.md) 的后端选型。

## 这个角色负责什么

| 负责 | 不负责 |
|---|---|
| 数据模型、API 契约、鉴权 | 页面与交互（前端角色） |
| 安全（参数化/加密/最小权限） | 部署与监控值班（DevOps 角色） |
| 文件存储方案、第三方 API 集成 | 定功能范围（PM 角色） |

**边界判定**：产出物是"数据与契约"（schema、API 文档、权限规则）；产出物是"页面"或"部署脚本"就越界了。

## 独立跑完「后端工作」的流程

| 步 | 做什么 | 产出 | 耗时 |
|---|---|---|---|
| 1 | 选后端形态（见下方默认答案） | 一行选型决定 + 理由 | 1 小时 |
| 2 | 从用户故事建模：实体/字段/关系/索引/权限 | schema（含"明确不建的"） | 半天 |
| 3 | API 契约：端点/参数/响应/错误码 | API 文档（前端可 mock） | 半天 |
| 4 | 实现：逐端点开发 + 单元测试（纯逻辑） | 可调通的接口 | 按任务卡 |
| 5 | 安全自检：走一遍 30 项清单 | 安全清单勾选记录 | 1 小时 |

**默认答案**：小程序 → 微信云开发（免域名免备案免运维，[官方文档](https://developers.weixin.qq.com/miniprogram/dev/wxcloud/)）；Web SaaS → Supabase（Postgres + Auth + Storage）。不满再换——自建服务器需要 HTTPS + ICP 备案 + 域名白名单三件套。三阶段演进观见 [backend.md](backend.md)。

## 收什么 → 交什么

| 上游交给我（PM 角色 / 环节 02） | 我交给下游 |
|---|---|
| PRD（用户故事 + 数据模型草案 + 非目标） | → 前端角色：API 契约（可 mock） |
| 平台约束（个人主体不能支付等） | → DevOps 角色：部署清单（环境变量/迁移/备份范围） |
| | → 财务角色：支付数据的存储口径 |

## 真实参考（不用凭空想象）

- 托管后端：[Supabase](https://supabase.com/)、[微信云开发](https://developers.weixin.qq.com/miniprogram/dev/wxcloud/)；极简自托管用 [PocketBase](https://github.com/pocketbase/pocketbase)（单二进制）
- 开源替代清单：[princepal9120/awesome-solo-founder-oss](https://github.com/princepal9120/awesome-solo-founder-oss)（★66）——数据库/鉴权/存储/邮件四类直接对应本页选型
- 支付（需企业主体）：[微信支付商户平台](https://pay.weixin.qq.com/)（规则以官方为准）
- 本仓库技能：[api-design](../../skills/backend/api-design.md) ✅ / [db-schema](../../skills/backend/db-schema.md) ✅ / [security-checklist](../../skills/backend/security-checklist.md) ✅

## 新手最容易踩的坑

| 坑 | 为什么会踩 | 怎么避 |
|---|---|---|
| SQL 字符串拼接 | 图快 | 参数化查询是底线，30 项清单第 1 组 |
| 密钥硬编码进仓库 | "本地跑跑没事" | 环境变量 + `.gitignore` 从第一个 commit 生效 |
| 过早分库分表/微服务 | 听架构播客听的 | <10 万用户用不到；三阶段演进而来 |
| 集合/表权限全开 | 云开发默认方便 | 默认最小权限，逐个集合/表写清谁可读写 |
| 接口永远返回 200 | 前端好处理 | 错误码规范先行（`api-design` 产出的一部分） |

## 本目录文件

| 文件 | 什么时候读 |
|---|---|
| [backend.md](backend.md) | 完整方法论：三阶段选型、数据库、鉴权、文件上传、OWASP Top 10 |
| [README.md](README.md)（本页） | 流程与契约 |

## 带走清单（独立使用本目录时）

- `skills/backend/` 三个技能（api-design / db-schema / security-checklist）
- `docs/decisions/0004-external-references-policy.md` —— 引用外部规则时的写法约定
- 上游输入格式：`docs/02-product/` 交出的 PRD 结构
