# 05 - 后端开发

## 独立开发者的后端选择

### 三个阶段

```
阶段 1: MVP（0-1k 用户）
  → BaaS：Supabase / Firebase / 微信云开发
  → Serverless：Vercel Functions / Cloudflare Workers
  
阶段 2: 增长（1k-100k 用户）
  → 单体应用：Node/Python/Go on Render/Railway/Fly.io
  → 仍可考虑 PaaS
  
阶段 3: 规模（100k+ 用户）
  → 微服务 / 多区域部署
  → 但大部分独立开发者到不了这阶段
```

### 推荐栈

#### 选项 A：JavaScript 全栈（最简单）
```
Next.js + Vercel + Supabase + Stripe
```
- 优势：单一语言，前后端复用
- 适合：Web SaaS

#### 选项 B：TypeScript 全栈（更稳）
```
Next.js + tRPC + Prisma + PostgreSQL
```
- 优势：端到端类型安全
- 适合：复杂 SaaS

#### 选项 C：Python 全栈
```
Django + PostgreSQL + Celery + Redis
```
- 优势：成熟、AI/ML 友好
- 适合：AI 产品

#### 选项 D：小程序后端
```
微信云开发（云函数 + 云数据库）
```
- 优势：免备案、免运维
- 适合：微信小程序

---

## 必备技能

### 基础
- HTTP / REST API
- 数据库（SQL + NoSQL）
- 鉴权（JWT / OAuth）
- 文件上传
- 邮件发送

### 进阶
- 缓存（Redis）
- 队列（Bull / Celery）
- 定时任务
- WebSocket
- 安全（XSS / CSRF / SQL 注入）

### 按需
- 搜索引擎（Elasticsearch）
- AI 集成（OpenAI API）
- 第三方 API（Stripe / 邮件 / SMS）

---

## 数据库选择

### SQL（关系型）
- **PostgreSQL** - 推荐，稳
- **MySQL** - 国内云标配
- **SQLite** - 极简项目

### NoSQL
- **MongoDB** - 文档型
- **Redis** - 缓存
- **DynamoDB** - AWS

### 嵌入式
- **SQLite** - 移动端

### 选型原则
- **关系型优先**：90% 场景用 SQL
- **不要为了 NoSQL 而 NoSQL**
- **不要过早分库分表**

---

## API 设计

### RESTful 最佳实践
```
GET    /users          列表
GET    /users/:id      详情
POST   /users          创建
PUT    /users/:id      完整更新
PATCH  /users/:id      部分更新
DELETE /users/:id      删除
```

### 命名规范
- 资源用复数：`/users`、`/orders`
- 用名词不用动词：`GET /users/123` 而不是 `GET /getUser`
- 用 kebab-case：`/user-profiles`

### 响应格式
```json
// 成功
{
  "data": { ... },
  "meta": { "total": 100 }
}

// 错误
{
  "error": {
    "code": "INVALID_INPUT",
    "message": "Email is required"
  }
}
```

### 工具
- **OpenAPI / Swagger**：API 文档
- **Postman / Insomnia**：测试
- **hoppscotch**：开源替代

---

## 鉴权

### 方案对比
| 方案 | 适合 |
|---|---|
| **JWT** | 自建 API |
| **Session + Cookie** | 传统 Web |
| **OAuth 2.0** | 第三方登录 |
| **微信开放平台** | 小程序 |
| **Clerk / Auth0** | 快速集成 |
| **Supabase Auth** | Supabase 项目 |

### JWT 实战
```js
// 签发
const token = jwt.sign({ userId: 123 }, SECRET, { expiresIn: '7d' });

// 验证
const payload = jwt.verify(token, SECRET);
```

---

## 文件上传

### 云存储选择
| 服务 | 价格 | 适合 |
|---|---|---|
| **AWS S3** | 便宜 | 大流量 |
| **Cloudflare R2** | 免费额度大 | 中小项目 |
| **Supabase Storage** | 免费 1GB | Supabase 项目 |
| **七牛云 / 又拍云** | 国内便宜 | 国内项目 |
| **微信云存储** | 免费额度 | 小程序 |

### 最佳实践
- **客户端直传**：避免服务器中转
- **预签名 URL**：安全授权
- **CDN 加速**：全球访问
- **图片处理**：缩略图、WebP、压缩

---

## 安全

### OWASP Top 10（独立开发者必看）

1. **注入**（SQL / 命令）
   - 使用参数化查询
   - 永远不要拼接 SQL

2. **身份认证失效**
   - 强密码、bcrypt
   - 多因素认证

3. **敏感数据泄露**
   - HTTPS everywhere
   - 数据库加密

4. **XXE**（XML 外部实体）
   - 禁用 XML 外部实体

5. **访问控制失效**
   - 默认拒绝，最小权限

6. **安全配置错误**
   - 不暴露 stack trace
   - 不暴露管理后台

7. **XSS**
   - 转义所有用户输入
   - Content Security Policy

8. **不安全反序列化**
   - 验证所有输入

9. **使用含漏洞组件**
   - npm audit / pip audit
   - 定期更新依赖

10. **日志和监控不足**
    - 集中日志
    - 异常告警

---

## 推荐学习资源

### 书
- 《数据密集型应用系统设计》（DDIA）
- 《Web 性能权威指南》
- 《HTTP 权威指南》

### 课程
- **boot.dev** - 后端工程师成长路径
- **ThePrimeagen** - 系统设计
- ** Hussein Nasser** - 后端细节

### 必订阅
- Console (newsletter by levelsio)
- ByteByteGo (系统设计)

