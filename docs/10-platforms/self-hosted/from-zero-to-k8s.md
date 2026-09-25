# 从零到生产：购买云服务器 → K8s → 用户用上你的 App

> **本文回答一个问题**：手里只有一个代码仓库，怎么一步步走到"用户打开就能用"。
> 全程 10 个阶段，按序执行；总耗时约 **5-8 个工作日**（大头是备案等待，可与开发并行）。
> 规则与价格随时间变化，**以各云厂商与微信官方文档为准**（[ADR-0004](../../decisions/0004-external-references-policy.md)）。
>
> 🤖 **想让我（或 Codex / Claude Code）全程代跑？**用配套的 [checklist-zero-to-prod.md](checklist-zero-to-prod.md)：填参数表 → AI 按带执行者标注的 CheckList 执行（支付/扫码/备案留给你的那部分都标了 👤）。

## 阶段 0：先决定要不要这条路（5 分钟，最重要）

本仓库的默认立场是 PaaS 优先（见 [../../06-devops/devops.md](../../06-devops/devops.md)）。自建 + K8s 是"能力更强、操心更多"的路线，先对号入座：

| 你的情况 | 走哪条路 |
|---|---|
| 纯前端 / Next.js 应用，海外用户 | Vercel / Cloudflare Pages，**别自建** |
| 微信小程序 + 简单后端 | 微信云开发，**别自建**（免域名免备案免运维） |
| Web SaaS，单体后端，用户 <1 万 | 单机 Docker Compose（见 [self-hosted.md](self-hosted.md)），**别上 K8s** |
| 多个服务要编排、要弹性伸缩、或学习 K8s 本身就是目标 | **本文路线** ✅ |

> 诚实的提醒：K8s 对单人项目是 80% 的运维开销换 20% 的收益。如果学习 K8s 是你的目标之一，本文用 **K3s**（轻量发行版，单二进制，1-3 节点够用）走完全程——概念与标准 K8s 一致，迁移几乎无痛。

## 阶段 1：购买云服务器（30 分钟）

**第一步决策：国内还是海外？**

| | 国内（阿里云/腾讯云） | 海外（任意 VPS） |
|---|---|---|
| ICP 备案 | **必须**（7-20 个工作日） | 不需要 |
| 国内用户访问 | 快 | 慢且不稳 |
| 小程序 request 域名 | 必须国内 + 备案 | ❌ 用不了 |
| 面向海外 SaaS | 一般 | 合适 |

**微信小程序的后端必须选国内**。海外产品选海外。

**购买清单**（以阿里云 ECS / 腾讯云 CVM 为例，控制台选项照此勾）：

1. **地域**：离目标用户近（国内用户选华东/华北大区）
2. **规格**：
   - Docker Compose 单机：2 核 4G 起步（约 ¥40-100/月）
   - K3s + 数据库 + Redis + 双应用：**4 核 8G 起步**（约 ¥150-300/月），2C4G 会 OOM
3. **操作系统**：Ubuntu 22.04 / 24.04 LTS
4. **登录方式：密钥对**（创建时就要选，别用密码）
5. **带宽**：固定带宽 3-5M 或按流量（个人项目按流量更省）
6. **磁盘**：40G 系统盘 + 建议加一块数据盘放数据库（重装系统不丢数据）

## 阶段 2：域名 + ICP 备案（并行启动，等待期与阶段 3-5 并行）

1. **买域名**：在与服务器**同一家厂商**买（备案方便）：[阿里云](https://www.aliyun.com) / [腾讯云](https://cloud.tencent.com)，完成域名实名认证
2. **ICP 备案**（仅国内服务器需要）：厂商控制台提交 → 管局审核，全程 **7-20 个工作日**。要点：
   - 期间域名**不能解析**到服务器（先规划好）
   - 网站底部要放备案号
   - 备案性质：个人备案不能放经营性内容
3. **公安备案**：ICP 通过后 30 天内到全国互联网安全管理服务平台办理
4. 官方入口以工信部备案系统为准（beian.miit.gov.cn），流程细节各厂商控制台有向导

> 备案期间干阶段 3-5 的所有活，用 IP 直连调试；解析等备案下来再配。

## 阶段 3：服务器初始化（1 小时）

拿到服务器 IP 和密钥后，SSH 上去做五件事（[self-hosted.md](self-hosted.md) 有完整版，这里是必做集）：

```bash
# 1. 用密钥登录（本地执行）
ssh -i ~/.ssh/your_key ubuntu@<服务器IP>

# 2. 基础安全（禁密码登录、防火墙、防爆破、自动安全更新）
sudo apt update && sudo apt upgrade -y
sudo apt install -y curl wget git ufw fail2ban unattended-upgrades chrony
sudo sed -i 's/PasswordAuthentication yes/PasswordAuthentication no/' /etc/ssh/sshd_config
sudo systemctl restart sshd
sudo ufw allow OpenSSH && sudo ufw allow 80 && sudo ufw allow 443 && sudo ufw enable

# 3. K8s 需要的内核参数
cat <<EOF | sudo tee /etc/sysctl.d/k3s.conf
net.ipv4.ip_forward = 1
EOF
sudo sysctl --system

# 4. 时区（日志时间戳一致性）
sudo timedatectl set-timezone Asia/Shanghai
```

**同时在云控制台做**：安全组/防火墙规则——22 端口限制为你自己的 IP（白名单），80/443 对公网开放，**数据库端口（5432/6379）永不开放公网**。

## 阶段 4：打包——前后端 Docker 化（半天）

**后端**（以 Node 为例，多阶段构建）：

```dockerfile
# Dockerfile
FROM node:20-alpine AS build
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

FROM node:20-alpine
WORKDIR /app
ENV NODE_ENV=production
COPY --from=build /app/dist ./dist
COPY --from=build /app/node_modules ./node_modules
COPY package*.json ./
EXPOSE 3000
USER node
CMD ["node", "dist/main.js"]
```

**前端**：`npm run build` 产出静态文件，两条路二选一：
- **A. Nginx 镜像**：`FROM nginx:alpine` + 把 `dist/` COPY 进 `/usr/share/nginx/html`（本文走这条）
- B. 对象存储 + CDN（流量大再升级）

**本地自测 → 推镜像仓库**：

```bash
docker build -t api ./backend && docker build -t web ./frontend
docker compose up   # 本地把前后端+数据库串起来点一遍，能跑再推

# 推到镜像仓库（阿里云 ACR / 腾讯云 TCR 个人版免费）
docker login registry.cn-hangzhou.aliyuncs.com
docker tag api registry.cn-hangzhou.aliyuncs.com/<命名空间>/api:v1.0.0
docker tag web registry.cn-hangzhou.aliyuncs.com/<命名空间>/web:v1.0.0
docker push registry.cn-hangzhou.aliyuncs.com/<命名空间>/api:v1.0.0
docker push registry.cn-hangzhou.aliyuncs.com/<命名空间>/web:v1.0.0
```

命名规范：`<服务名>:<语义化版本>`，**不用 latest**（回滚时要精确版本号）。

## 阶段 5：安装 K8s——实用主义路线 K3s（1 小时）

```bash
# 一条命令装完（含容器运行时、Traefik Ingress、CoreDNS、本地存储）
curl -sfL https://get.k3s.io | sh -s - --write-kubeconfig-mode 644

# 验证：两个 pod Ready 即成功
sudo kubectl get nodes
sudo kubectl get pods -n kube-system

# 本地机器管理远程集群：把 /etc/rancher/k3s/k3s.yaml 拷到本地 ~/.kube/config
# （并 把其中的 127.0.0.1 改成服务器公网 IP）
kubectl get nodes   # 本地能看到节点 = 管理通道打通
```

- 安装文档：[docs.k3s.io/quick-start](https://docs.k3s.io/quick-start)（以官方为准）
- 要学标准发行版（kubeadm 自建控制面）另走 [Kubernetes 官方文档](https://kubernetes.io/docs/home/)——概念相同，本文不展开
- 卸载后路：`/usr/local/bin/k3s-uninstall.sh`（装错了随时重来）

## 阶段 6：中间件（半天）

**先做一个决策**：数据库放哪？

| 方案 | 月成本 | 适合 |
|---|---|---|
| **In-cluster（Helm 装 bitnami postgresql）** | ¥0 | 单人 + 小项目，本文默认 |
| 云数据库 RDS（托管） | +¥100-300/月 | 怕运维、想要自动备份高可用 |

In-cluster 路线：

```bash
curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash   # 装 Helm ([helm.sh](https://helm.sh))
helm repo add bitnami https://charts.bitnami.com/bitnami
helm install pg bitnami/postgresql -n data --create-namespace \
  --set auth.database=app --set auth.username=app --set auth.password=<强密码>
helm install redis bitnami/redis -n data --set auth.password=<强密码>
```

**装完立刻做三件事**（不做等于裸奔）：
1. 密码只进 Secret（下阶段），不进 Git
2. 数据库/Redis 的 Service 保持 ClusterIP（**绝不开公网**）
3. 备份 cron：`pg_dump` 每日一次 + 异地上传（恢复演练清单见 `skills/devops/backup-strategy`）

对象存储（用户上传文件）：云厂商 OSS/COS，或自建 [MinIO](https://min.io)。

## 阶段 7：配置初始化（半天）

```bash
kubectl create namespace app

# Secret：所有密码与密钥（base64 只是编码不是加密，控制好 RBAC 即可）
kubectl -n app create secret generic app-secrets \
  --from-literal=DATABASE_URL=postgres://app:<密码>@pg-postgresql.data.svc.cluster.local:5432/app \
  --from-literal=JWT_SECRET=<随机串> \
  --from-literal=WX_APPID=<小程序appid> \
  --from-literal=WX_SECRET=<小程序secret>

# ConfigMap：非敏感配置
kubectl -n app create configmap app-config --from-literal=NODE_ENV=production --from-literal=LOG_LEVEL=info
```

数据库表结构初始化/升级用 **Job**（不放进应用启动流程）：

```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: db-migrate-v1
  namespace: app
spec:
  template:
    spec:
      restartPolicy: Never
      containers:
        - name: migrate
          image: registry.cn-hangzhou.aliyuncs.com/<ns>/api:v1.0.0
          command: ["npx", "prisma", "migrate", "deploy"]   # 换成你的迁移命令
          envFrom:
            - secretRef: { name: app-secrets }
```

## 阶段 8：部署前后端（1 天）

**后端 Deployment**（关键字段都注释了）：

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: api
  namespace: app
spec:
  replicas: 2
  selector:
    matchLabels: { app: api }
  template:
    metadata:
      labels: { app: api }
    spec:
      containers:
        - name: api
          image: registry.cn-hangzhou.aliyuncs.com/<ns>/api:v1.0.0
          ports: [{ containerPort: 3000 }]
          envFrom:
            - secretRef: { name: app-secrets }
            - configMapRef: { name: app-config }
          resources:                # 不写 limits 容易互相 OOM
            requests: { cpu: 250m, memory: 256Mi }
            limits:   { cpu: "1",  memory: 512Mi }
          readinessProbe:           # 没就绪不接流量
            httpGet: { path: /healthz, port: 3000 }
            initialDelaySeconds: 5
          livenessProbe:            # 挂了自动重启
            httpGet: { path: /healthz, port: 3000 }
            initialDelaySeconds: 15
---
apiVersion: v1
kind: Service
metadata:
  name: api
  namespace: app
spec:
  selector: { app: api }
  ports: [{ port: 3000, targetPort: 3000 }]   # ClusterIP，仅集群内可达
```

前端同理（镜像换 web，端口 80），Service 命名 `web`。

**HTTPS 入口**（cert-manager 自动签 Let's Encrypt）：

```bash
kubectl apply -f https://github.com/cert-manager/cert-manager/releases/latest/download/cert-manager.yaml
```

```yaml
apiVersion: cert-manager.io/v1
kind: ClusterIssuer
metadata: { name: letsencrypt }
spec:
  acme:
    server: https://acme-v02.api.letsencrypt.org/directory
    email: you@example.com
    privateKeySecretRef: { name: letsencrypt-key }
    solvers: [{ http01: { ingress: { class: traefik } } }]
---
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: app-ingress
  namespace: app
  annotations: { cert-manager.io/cluster-issuer: letsencrypt }
spec:
  tls: [{ hosts: [app.example.com], secretName: app-tls }]
  rules:
    - host: app.example.com
      http:
        paths:
          - path: /api
            pathType: Prefix
            backend: { service: { name: api, port: { number: 3000 } } }
          - path: /
            pathType: Prefix
            backend: { service: { name: web, port: { number: 80 } } }
```

证书自动签发 + 到期自动续；`https://app.example.com` 通了就成功。

## 阶段 9：域名解析与白名单收口（1 小时）

1. **DNS**：域名控制台加 A 记录 `app.example.com → 服务器公网 IP`（备案已下来的前提下）
2. **安全组白名单核对**（收口清单）：
   - [ ] 22 端口：仅你的 IP
   - [ ] 80/443：0.0.0.0/0
   - [ ] 5432/6379/6443：**不开放**
   - [ ] 服务器本机 ufw 与云安全组规则一致
3. **微信小程序合法域名**（小程序路线）：MP 后台 → 开发管理 → 服务器域名，把 `https://app.example.com` 加进 request 合法域名。注意：**必须 HTTPS + 已备案**，每月修改次数有限（以官方后台为准），**一次配齐再保存**
4. 验证链路：`curl https://app.example.com/api/healthz` 返回 200

## 阶段 10：CI/CD 与验收——让用户用上（1 天）

**代码仓库与 CI 平台怎么选**（GitHub vs GitLab 是高频问题）：

| 方案 | 适合 | 一句话 |
|---|---|---|
| **GitHub + Actions**（本文默认） | 已在 GitHub、个人项目、AI 工具链 | 公开仓库标准 runner 免费；私有仓每月 2000 分钟免费额度（以官方为准），单人项目足够 |
| GitLab.com + CI | 熟悉 GitLab CI 语法 | 跨境问题与 GitHub 相同，无额外收益 |
| GitLab CE 自托管 | 有合规要求必须代码自持 | **GitLab 本体就要 4G+ 内存，别和 K3s 抢同一台 4C8G**——真要自托管得再加一台 |
| 国内一站式（云效 / 腾讯 CODING / Gitee Go） | 团队在国内、想要全中文平台 | 免费额度够个人用，但 AI 工具链与开源生态都偏 GitHub |

**跨境问题的正解：自托管 runner**。GitHub Actions 的云端 runner 在海外，SSH 到国内服务器部署偶发超时。解法是把 GitHub runner 装在**部署服务器本机**（免费、不限时长）：构建推镜像走海外 runner，`deploy` 这个 job 标 `runs-on: [self-hosted]` 在你服务器上本地执行 `kubectl set image`——稳定且零延迟：

```bash
# GitHub 仓库 → Settings → Actions → Runners → New self-hosted runner
# 在部署服务器上按页面指引执行（systemd 服务方式安装），配 label: deploy
```

**CI/CD**（GitHub Actions，简单可靠优先，不引 ArgoCD）：

```yaml
# .github/workflows/deploy.yml 核心步骤
name: Deploy
on: { push: { branches: [main] } }
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: npm ci && npm test        # 先过测试
      - name: Build & push image
        run: |
          echo "${{ secrets.REGISTRY_PASSWORD }}" | docker login registry.cn-hangzhou.aliyuncs.com -u ${{ secrets.REGISTRY_USERNAME }} --password-stdin
          docker build -t registry.cn-hangzhou.aliyuncs.com/<ns>/api:${{ github.sha }} ./backend
          docker push registry.cn-hangzhou.aliyuncs.com/<ns>/api:${{ github.sha }}
      - name: Rollout on server
        uses: appleboy/ssh-action@v1
        with:
          host: ${{ secrets.SERVER_IP }}
          username: ubuntu
          key: ${{ secrets.SSH_KEY }}
          script: |
            sudo kubectl -n app set image deployment/api api=registry.cn-hangzhou.aliyuncs.com/<ns>/api:${{ github.sha }}
            sudo kubectl -n app rollout status deployment/api --timeout=120s
```

**上线验收清单**（全绿才发给用户）：
- [ ] `https://app.example.com` 浏览器正常 + 证书有效
- [ ] 核心路径走通：注册 → 核心功能 → 退出再进（数据持久化验证）
- [ ] 杀掉一个 api pod，服务不中断（滚动更新生效）
- [ ] 监控就位：拨测（Uptime Kuma）+ 错误上报（Sentry/微信运维中心）
- [ ] 备份跑过一次并演练恢复
- [ ] 小程序：体验版全功能回归 → 提审 → 发布

**把产品递到用户手里**：
- 小程序：审核通过后全量发布 → 用户搜一搜/扫码即达（增长见 [环节 07](../../../phases/07-launch/README.md)）
- Web：域名即入口 → 落地页 + 各渠道分发

## 全程成本参考（月）

| 项 | 规格 | 月费 |
|---|---|---|
| 服务器 | 4C8G 国内 | ¥150-300 |
| 域名 | .com | ¥60/年 |
| 镜像仓库 | ACR/TCR 个人版 | ¥0 |
| 数据库/Redis | In-cluster | ¥0（RDS 另加 ¥100-300） |
| HTTPS 证书 | Let's Encrypt | ¥0 |
| 备份存储 | OSS 一小块 | ¥1-10 |

## 新手最容易踩的 10 个坑

1. **买了海外服务器才想起做小程序**——小程序 request 域名必须国内 + 备案，购买前先定平台
2. **备案没下来就配解析**——备案期间域名不能解析，先把解析留空
3. **安全组开了 5432/6379 图方便**——数据库公网裸奔，扫描器 24 小时内就会到
4. **2C4G 跑 K3s 全家桶**——组件接连 OOMKilled，还以为是自己 YAML 写错了
5. **不写 resources limits**——一个服务吃光内存，全部被杀
6. **镜像用 latest tag**——回滚不知道该滚回哪
7. **迁移塞进应用启动**——多副本并发迁移互相踩；用 Job，先迁移后发版
8. **小程序合法域名分好几次改**——每月修改次数有限，一次配齐
9. **cert-manager 签不出证书**——90% 是 DNS 还没生效或 80 端口没开（HTTP-01 验证走 80）
10. **只备份不演练**——能备份 ≠ 能恢复，每季度走一遍 `skills/devops/backup-strategy` 的演练清单

## 与仓库其他文档的关系

- 平台总览：[README](../README.md)；手工单机路线（Docker Compose，更简单）：[self-hosted.md](self-hosted.md)
- 上线检查与监控：[../../06-devops/devops.md](../../06-devops/devops.md)、[phases/06-release](../../../phases/06-release/README.md)
- 事故与备份技能：`skills/devops/`（incident-response / backup-strategy / deploy-checklist）
- 让用户用上之后：[phases/07-launch](../../../phases/07-launch/README.md)（增长）与 [phases/08-iterate](../../../phases/08-iterate/README.md)（迭代变现）
