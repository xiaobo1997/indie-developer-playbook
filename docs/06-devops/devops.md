# 06 - DevOps 运维

## 独立开发者的 DevOps 原则

### 1. 用 PaaS 而不是 IaaS
- **不要自己管服务器**（除非必要）
- **不要学 Kubernetes**（除非必要）
- **不要优化过早**

### 2. 自动化一切
- CI/CD 自动部署
- 监控告警自动化
- 备份自动化

### 3. 简单可靠 > 高级
- 一个服务能搞定就不要拆
- 单库能搞定就不要分库
- 单机能搞定就不要集群

---

## 推荐部署平台（按简单程度）

### Tier 1：最简单（推荐起步）

#### Vercel
- **适合**：Next.js / React / Vue
- **优点**：零配置，自动 HTTPS，全球 CDN
- **价格**：免费额度够用
- **网站**：https://vercel.com

#### Netlify
- **适合**：静态站 / Jamstack
- **价格**：免费额度大
- **网站**：https://netlify.com

#### Cloudflare Pages
- **适合**：静态站 + Workers
- **优点**：极快 CDN，价格便宜
- **价格**：免费额度很大
- **网站**：https://pages.cloudflare.com

### Tier 2：稍微复杂

#### Render
- **适合**：Node/Python/Ruby/Go Web
- **优点**：零配置部署
- **价格**：免费起步
- **网站**：https://render.com

#### Railway
- **适合**：全栈应用
- **优点**：数据库一体化
- **网站**：https://railway.app

#### Fly.io
- **适合**：全球部署
- **优点**：边缘计算
- **网站**：https://fly.io

### Tier 3：需要运维

#### DigitalOcean
- **适合**：VPS / Kubernetes
- **价格**：$4/月起
- **网站**：https://digitalocean.com

#### 阿里云 / 腾讯云
- **适合**：国内项目
- **优点**：国内访问快
- **缺点**：配置复杂

---

## CI/CD

### GitHub Actions（推荐）
```yaml
# .github/workflows/deploy.yml
name: Deploy
on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: 20
      - run: npm ci
      - run: npm run build
      - run: npm test
      - uses: vercel-action@v25
        with:
          vercel-token: ${{ secrets.VERCEL_TOKEN }}
```

### 必备自动化
- [ ] 测试通过自动部署
- [ ] Lint 自动检查
- [ ] 依赖更新自动 PR（Dependabot）
- [ ] 安全扫描（npm audit）
- [ ] 自动打 tag（语义化版本）

---

## 监控

### 必须监控的指标

#### 应用层
- 错误率（Error rate）
- 响应时间（Latency）
- 流量（Throughput）

#### 业务层
- DAU / WAU / MAU
- 转化率
- MRR / ARR
- 流失率

#### 系统层
- CPU / 内存 / 磁盘
- 网络流量
- 数据库连接

### 推荐工具

#### 错误追踪
| 工具 | 价格 | 适合 |
|---|---|---|
| **Sentry** | 免费额度 | 全平台，开源 |
| **Rollbar** | 付费 | 大型项目 |
| **Bugsnag** | 付费 | 移动端 |

#### 性能监控 (APM)
- **Datadog** - 全功能，贵
- **New Relic** - 老牌
- **Sentry Performance** - 集成在 Sentry
- **Plausible** - 隐私友好分析

#### 系统监控
- **UptimeRobot** - 免费，基础
- **BetterStack** - 现代
- **自建 Prometheus + Grafana** - 高级

#### 业务分析
- **PostHog** - 产品分析
- **Plausible** - 网站分析
- **Umami** - 自托管
- **Mixpanel** - 事件分析

---

## 数据库运维

### 备份策略

#### 3-2-1 法则
- 3 份副本
- 2 种介质
- 1 个异地

#### Supabase / Neon
- 自动每日备份
- 一键恢复
- Point-in-time recovery

#### 自建 PostgreSQL
```bash
# 每日自动备份
0 2 * * * pg_dump dbname | gzip > /backup/db-$(date +\%Y\%m\%d).sql.gz

# 上传到 S3
aws s3 cp /backup/db-*.sql.gz s3://my-backups/db/
```

### 性能优化
- 索引（覆盖最常用查询）
- 查询优化（EXPLAIN ANALYZE）
- 连接池（PgBouncer）
- 读写分离（流量大时）

---

## 安全清单

### 上线前必查
- [ ] HTTPS 强制
- [ ] HSTS 头
- [ ] CSP 头
- [ ] X-Frame-Options
- [ ] 输入验证
- [ ] 输出转义
- [ ] CSRF 保护
- [ ] 速率限制
- [ ] 错误信息不泄露细节
- [ ] 依赖无已知漏洞（npm audit）
- [ ] 密钥在环境变量（不在代码里）
- [ ] 数据库连接用 SSL
- [ ] 备份正常

### 推荐安全工具
- **Mozilla Observatory** - 安全评分
- **Snyk** - 依赖漏洞扫描
- **GitHub Dependabot** - 自动更新
- **Aikido Security** - 综合扫描

---

## 域名与 DNS

### 域名注册商
| 平台 | 价格 | 适合 |
|---|---|---|
| **Cloudflare Registrar** | 成本价 | 推荐 |
| **Porkbun** | 便宜 | 备选 |
| **Namecheap** | 中等 | 老牌 |
| **GoDaddy** | 贵 | 不推荐 |
| **腾讯云 / 阿里云** | 国内 | 国内备案需要 |

### DNS 服务
- **Cloudflare DNS** - 免费，极快
- **Route 53** - AWS
- **DNSPod** - 国内

---

## 成本控制

### 独立开发者典型月成本

#### 起步（$0-50/月）
- Vercel/Netlify: 免费
- Supabase: 免费
- Cloudflare: 免费
- 域名: $10/年

#### 增长（$50-500/月）
- Vercel Pro: $20/月
- Supabase Pro: $25/月
- Sentry: $26/月
- 服务器: $50-100/月

#### 规模（$500+/月）
- 多个服务
- CDN
- 数据库
- 第三方服务

### 省钱技巧
- 免费额度用足
- 先优化再扩容
- 用开源替代商业服务
- 避免云厂商锁定

