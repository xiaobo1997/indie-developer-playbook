# 部署配置清单与执行 CheckList（人机协作版）

> **用法**：你填 [参数表](#一参数表先填这个) → 把本文件交给任何能执行命令/操作浏览器的 AI 代理（ZCode / Codex / Claude Code）→ AI 按 CheckList 从头执行或从断点继续。
> 每个条目标注执行者：**👤 人**（支付/实名/扫码/身份验证，AI 不做）/ **🤖 AI**（终端命令、写配置、浏览器操作）/ **👥 协作**（人看着、AI 动手）。
> 路线细节（命令与 YAML 全文）见 [from-zero-to-k8s.md](from-zero-to-k8s.md)——本文件是执行骨架，遇详情跳过去。

## 敏感信息规则（先读，30 秒）

| 类型 | 规则 |
|---|---|
| 💰 支付/下单 | **永远 👤 人**。AI 给精确配置单，人不确认就不买 |
| 🔑 密码/Secret/私钥 | **不进聊天、不进 Git**。由 AI 在服务器上生成并直接写入 Secret/文件，只汇报"已设置"；人要保留的抄进自己的密码管理器 |
| 📱 短信验证码/人脸/扫码 | **永远 👤 人**。AI 停下给出指引 |
| AppID 这类非密钥标识 | 可以写进参数表 |
| AI 禁止项 | 清单外的破坏性命令（rm -rf 类、drop database、安全组全开）一律先停下确认 |

## 自动化通道：能 API 的绝不开浏览器（先读，1 分钟）

**主路径零浏览器**。React 控制台对浏览器自动化不友好（虚拟 DOM、弹层、扫码墙），所以执行架构按此优先级：**CLI > OpenAPI > 浏览器（兜底）**。云厂商全量 OpenAPI 都有官方 CLI 包着，浏览器只在极少数兜底场景出现：

| 通道 | 覆盖范围 | 前置（一次性） |
|---|---|---|
| `aliyun` CLI / `tccli` | ECS 买卖、安全组、DNS 解析、镜像仓、SSL……全部 OpenAPI | 人创建 **RAM 子账号**（别用主账号 AK）→ 只授权部署所需权限 → AK 交给 AI 配进 CLI |
| `gh` CLI / GitHub API | 仓库、Secrets、Actions、runner 注册 | 人 `gh auth login` 一次 |
| `kubectl` / `helm` | 集群内一切（部署、迁移、回滚、证书） | 无（K3s 装完即得） |
| 微信小程序 API | **服务器域名设置等管理操作**（`modify_domain` 等，[官方文档](https://developers.weixin.qq.com/miniprogram/dev/OpenApiDoc/domain-manager/modify-domain.html)） | 人做两件事：MP 后台把**调用方 IP 白名单**配好、提供 appid/secret（secret 走 🔒 规则）；此后 AI 调 API 改域名，月度限额照旧以官方为准 |

**真正绕不开人的只剩一次性 4 件事**（法律/身份/资金层，任何工具都不该代劳）：
1. 云账号注册 + 实名认证 + 充值/开通（若选择"AI 用 RunInstances API 建机"路线，人只需[确保余额](https://help.aliyun.com/zh/ecs/developer-reference/runinstances)；否则照配置单手动下单）
2. **ICP 备案 + 公安备案**（政府流程：身份材料、可能人脸/签名）
3. 微信 MP 后台：管理员扫码登录一次 + 配 IP 白名单
4. 域名购买与实名（也可 API 买，实名仍需一次）

**结论**：这 4 件一次性做完之后，从建服务器到上线发布的**全流程 AI 无头执行**，无浏览器依赖。

---

## 一、参数表（先填这个）

复制到你的项目里填好，执行时把路径交给 AI（🔒 行留空，执行到对应步骤时现场处理）：

```yaml
# deploy-params.yaml
product:
  name: ""                        # 产品名，如 "单词卡"
  platform: ""                    # miniprogram / web / both
  repo_url: ""                    # git@github.com:xiaobo1997/xxx.git（私有仓）
  backend_stack: ""               # 如 node20+prisma+postgres / python3.12+fastapi
  frontend_stack: ""              # 如 vue3+vite / next14
  repo_layout: ""                 # monorepo(backend/,frontend/) / 单仓
cloud:
  vendor: ""                      # aliyun / tencent（国内必须，备案前提）
  account_ready: false            # 已注册并实名认证？（👤）
  region: ""                      # 如 cn-hangzhou（离用户近）
  server_spec: "4c8g"             # K3s 路线最低 4c8g
  server_ip: ""                   # 购买后回填
  ssh_key_path: "~/.ssh/deploy_key"
domain:
  domain: ""                      # example.com
  bought: false                   # （👤）
  icp_status: ""                  # not_started / in_review / approved
  subdomain_app: "app"            # app.example.com
registry:
  vendor: ""                      # 跟云厂商同家（个人版免费）
  namespace: ""                   # 命名空间名
wechat:                           # platform 含 miniprogram 才需要
  appid: ""                       # 可填
  secret: ""                      # 🔒 留空，F-03 时在服务器上处理
  request_domain_set: false       # MP 后台合法域名（H-03，👤扫码）
alerts:
  email: ""                       # 接收监控告警
budget:
  monthly_cap_cny: 0              # 月预算上限，AI 的所有建议不得超过
cd:
  github_secrets_ready: false     # I-01 时处理
notes: ""                         # 其他约定
```

---

## 二、主 CheckList

> 断点续跑：告诉 AI「从 D-03 继续」即可。每完成一项打 ✅ 并记录验证证据。

### A. 准备（开工前）

| # | 事项 | 谁 | 完成标准 |
|---|---|---|---|
| A-01 | 确定平台路线与预算上限 | 👤 | 参数表 platform / budget 已填 |
| A-02 | 云厂商注册 + 实名认证 + 充值（或直接手动下单） | 👤 | 控制台可登录，余额 ≥ 1 个月预算 |
| A-03 | 创建 RAM 子账号 + AK，只授部署所需权限，AK 交 AI 配 CLI（CLI 手册：[aliyun](https://help.aliyun.com/zh/cli/install-aliyun-cli) / [tccli](https://cloud.tencent.com/document/product/440/35894)） | 👥 | `aliyun ecs DescribeInstances` 能通 |
| A-04 | 填完参数表（🔒 行除外） | 👤 | 文件存在，AI 可读 |

### B. 购买服务器

| # | 事项 | 谁 | 完成标准 |
|---|---|---|---|
| B-01 | AI 按参数表生成购买配置单（地域/规格/OS/密钥对/带宽/磁盘/价格区间） | 🤖 | 配置单经人确认 |
| B-02 | 两条路任选：**API 建机**（AI 调 `RunInstances`，走 A-02 的余额）/ 人按单下单支付 | 👥/👤 | 控制台实例"运行中" |
| B-03 | 回填公网 IP 到参数表；API 路线由 AI 创建并绑定 SSH 密钥对 | 👥 | `ssh -i <key> ubuntu@<ip>` 由 AI 验证可登录 |
| B-04 | 本地 `~/.ssh/config` 配好主机别名 | 🤖 | `ssh <alias>` 直达 |

### C. 域名与备案（等待期与 D-G 并行）

| # | 事项 | 谁 | 完成标准 |
|---|---|---|---|
| C-01 | 购买域名（同厂商） | 👤 | 域名列表可见 |
| C-02 | 域名实名认证 | 👤 | 状态"已实名" |
| C-03 | 提交 ICP 备案（AI 可代写"网站名称/服务内容"描述，人核对提交） | 👥 | 备案号或"审核中" |
| C-04 | 公安备案（ICP 通过后 30 天内） | 👤 | 完成回执 |

### D. 服务器初始化

| # | 事项 | 谁 | 完成标准 |
|---|---|---|---|
| D-01 | SSH 连通性验证 | 🤖 | 登录成功，`uname -a` 正常 |
| D-02 | 安全基线：apt 升级、禁密码登录、ufw(22/80/443)、fail2ban、unattended-upgrades、chrony | 🤖 | 脚本跑完，`ufw status` 符合预期 |
| D-03 | 内核参数（ip_forward）+ 时区 Asia/Shanghai | 🤖 | `sysctl net.ipv4.ip_forward` = 1 |
| D-04 | 云安全组：22 限本人 IP、80/443 公网、5432/6379/6443 不开（AI 调安全组 API 改，A-03 的 RAM 权限需含 ECS 安全组写权限） | 🤖 | 与 D-02 一致，双保险 |
| D-05 | 复查：外部端口扫描只应见 22/80/443 | 🤖 | 扫描结果符合预期 |

### E. 打包与镜像

| # | 事项 | 谁 | 完成标准 |
|---|---|---|---|
| E-01 | 后端 Dockerfile（多阶段）+ .dockerignore | 🤖 | 本地 `docker build` 成功 |
| E-02 | 前端构建产物 + Nginx 镜像 | 🤖 | 本地 build 成功 |
| E-03 | 本地 docker compose 串前后端+DB 自测 | 🤖 | 核心路径在本地走通 |
| E-04 | 开通镜像仓库个人版 | 👥 | 命名空间可推送 |
| E-05 | push 前后端镜像（语义化 tag，如 v1.0.0） | 🤖 | 仓库里可见两个镜像 |
| E-06 | 服务器 docker login（临时凭证） | 🤖 | 服务器可拉取 |

### F. K3s 与中间件

| # | 事项 | 谁 | 完成标准 |
|---|---|---|---|
| F-01 | 安装 K3s + kubeconfig 拷回本地（IP 替换 127.0.0.1） | 🤖 | `kubectl get nodes` Ready |
| F-02 | Helm + bitnami postgresql/redis（装到 data ns） | 🤖 | 两个 pod Running |
| F-03 | 生成强密码并在服务器上直接创建 Secret（不经聊天/Git） | 🤖 | `kubectl get secret` 存在；人把密码抄进自己的密码管理器 |
| F-04 | 数据库每日备份 cron + 首次备份执行 | 🤖 | 备份文件存在且可读 |
| F-05 | 集群内连通性：从 app ns 解析并连上 pg | 🤖 | `psql SELECT 1` 通过 |

### G. 部署前后端

| # | 事项 | 谁 | 完成标准 |
|---|---|---|---|
| G-01 | 数据库迁移 Job 跑通 | 🤖 | Job Complete，表已建 |
| G-02 | 后端 Deployment（2 副本/探针/资源限额）+ Service | 🤖 | pod 全 Ready，`/healthz` 200 |
| G-03 | 前端 Deployment + Service | 🤖 | pod 全 Ready |
| G-04 | cert-manager + ClusterIssuer + Ingress（备案前用临时自签，备案后切 Let's Encrypt） | 🤖 | 阶段性：IP/临时域名可访问 |
| G-05 | 韧性验证：杀一个 api pod，服务不断、自动拉起 | 🤖 | rollout/自愈正常 |

### H. 网络与域名收口

| # | 事项 | 谁 | 完成标准 |
|---|---|---|---|
| H-01 | DNS A 记录 `app.domain → 服务器IP`（备案通过后；AI 调 DNS API 改，兜底才用控制台） | 🤖 | `dig` 生效 |
| H-02 | Let's Encrypt 证书签发成功 | 🤖 | 浏览器 https 无告警 |
| H-03 | 小程序 request 合法域名：**AI 调微信 `modify_domain` API**（前置：人已在 MP 后台配 IP 白名单、交出 🔒 secret）；兜底才是人扫码手配。月度限额以官方为准 | 👥 | 后台/接口返回已配置 |
| H-04 | 全链路连通性：本地→域名→Ingress→Service→Pod→DB 各跳验证 | 🤖 | 每跳有证据 |
| H-05 | 安全组终审（对照 D-04 清单；AI 调安全组 API 核对） | 🤖 | 全部符合 |

### I. CI/CD

| # | 事项 | 谁 | 完成标准 |
|---|---|---|---|
| I-01 | GitHub Secrets：REGISTRY_USERNAME/PASSWORD、SERVER_IP、SSH_KEY（人粘贴，AI 列清单） | 👥 | secrets 列表齐全 |
| I-02 | deploy.yml：test → build → push → ssh rollout | 🤖 | 文件就位 |
| I-03 | 首次流水线全绿 | 🤖 | Actions ✅，线上版本变化 |
| I-04 | 回滚演练：`rollout undo` 回上一版本 | 🤖 | 版本回退成功再推回 |

### J. 验收与发布

| # | 事项 | 谁 | 完成标准 |
|---|---|---|---|
| J-01 | 冒烟清单：注册→核心功能→退出重进（数据持久化） | 👥 | 全部通过 |
| J-02 | 监控：拨测（Uptime Kuma）+ 错误上报（Sentry / 微信运维中心），告警触达 alerts.email | 🤖 | 测试告警能收到 |
| J-03 | 小程序体验版回归 → 提审 | 👤 | 提审中 |
| J-04 | 审核通过 → 全量发布；Web 直接开放 | 👤 | **用户开始使用** 🎉 |

---

## 三、怎么发起执行

把下面这段话发给 AI 代理（Codex / Claude Code / ZCode），附上参数表路径：

```text
读取 docs/10-platforms/self-hosted/checklist-zero-to-prod.md 与 deploy-params.yaml，
作为执行规约开始部署：
1. 从最早的未完成项继续；每完成一项在本文件勾选并附验证证据
2. 遇到 👤 项：停下来给我逐步指引，等我确认后再继续
3. 遇到 🔒 项：在服务器上现场生成/写入，不在对话里输出明文
4. 清单外的破坏性命令（删除、清库、改安全组为全开）必须先停下问我
5. 全程支出不超过参数表的 monthly_cap_cny
```

**执行中的四类停机点**（AI 必须停、你来做的，均为一次性）：账号实名与充值 / ICP 备案资料提交 / MP 管理员扫码 + 配 IP 白名单 / 任何超出预算上限的购买。其余全程 AI 无头执行——CLI 与 OpenAPI 优先，浏览器只是兜底（详见「自动化通道」节）。

## 四、与仓库其他文档的关系

- 路线全文（每个阶段为什么这样做、命令与 YAML 逐行）：[from-zero-to-k8s.md](from-zero-to-k8s.md)
- 更简单的单机 Compose 路线：[self-hosted.md](self-hosted.md)
- 上线前检查（应用层）：[../../../skills/devops/deploy-checklist.md](../../../skills/devops/deploy-checklist.md)
- 事故响应与备份演练：[../../../skills/devops/](../../../skills/README.md)
