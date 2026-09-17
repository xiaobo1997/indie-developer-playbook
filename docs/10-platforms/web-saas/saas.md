# Web SaaS 开发指南

## 适合谁

- 想做全球化产品
- 习惯订阅制
- B2B 业务
- 海外用户为主

## 技术栈推荐

### 现代 SaaS 主流：T3 Stack
```
Next.js 14+ (App Router)
TypeScript
tRPC（端到端类型安全）
Prisma + PostgreSQL
NextAuth.js（鉴权）
Tailwind CSS + shadcn/ui
```

### 简化版：Next.js + Supabase
```
Next.js 14+ (App Router)
TypeScript
Supabase（DB + Auth + Storage）
Tailwind CSS + shadcn/ui
```

### 全栈 Serverless
```
Next.js
TypeScript
Vercel
PlanetScale / Neon（DB）
Clerk / Auth0
Stripe
```

---

## 项目结构（T3 Stack）

```
saas-app/
├── prisma/
│   ├── schema.prisma
│   └── migrations/
├── src/
│   ├── app/                 # Next.js App Router
│   │   ├── api/
│   │   ├── (auth)/
│   │   ├── (dashboard)/
│   │   └── page.tsx
│   ├── server/
│   │   ├── api/            # tRPC routers
│   │   ├── auth.ts
│   │   └── db.ts
│   ├── components/
│   ├── lib/
│   ├── styles/
│   └── trpc/
├── public/
├── tests/
└── package.json
```

---

## 关键模块

### 1. 鉴权
```ts
// NextAuth.js
import NextAuth from "next-auth";
import GitHubProvider from "next-auth/providers/github";

export const authOptions = {
  providers: [
    GitHubProvider({
      clientId: process.env.GITHUB_ID!,
      clientSecret: process.env.GITHUB_SECRET!,
    }),
  ],
};

export default NextAuth(authOptions);
```

### 2. 数据库
```prisma
// schema.prisma
model User {
  id        String   @id @default(cuid())
  email     String   @unique
  name      String?
  createdAt DateTime @default(now())
  
  projects  Project[]
}

model Project {
  id        String   @id @default(cuid())
  name      String
  userId    String
  user      User     @relation(fields: [userId], references: [id])
  createdAt DateTime @default(now())
}
```

### 3. API（tRPC）
```ts
// server/api/routers/projects.ts
export const projectsRouter = createTRPCRouter({
  getAll: protectedProcedure.query(async ({ ctx }) => {
    return ctx.db.project.findMany({
      where: { userId: ctx.session.user.id }
    });
  }),
  
  create: protectedProcedure
    .input(z.object({ name: z.string() }))
    .mutation(async ({ ctx, input }) => {
      return ctx.db.project.create({
        data: {
          name: input.name,
          userId: ctx.session.user.id,
        },
      });
    }),
});
```

### 4. 支付（Stripe）
```ts
// app/api/webhook/stripe/route.ts
import Stripe from "stripe";

const stripe = new Stripe(process.env.STRIPE_SECRET_KEY!);

export async function POST(req: Request) {
  const body = await req.text();
  const sig = req.headers.get("stripe-signature")!;
  
  const event = stripe.webhooks.constructEvent(
    body, sig, process.env.STRIPE_WEBHOOK_SECRET!
  );
  
  switch (event.type) {
    case "checkout.session.completed":
      // 处理订阅成功
      break;
    case "invoice.payment_failed":
      // 处理付款失败
      break;
  }
  
  return new Response("ok");
}
```

---

## 部署

### Vercel（推荐）
```bash
npm install -g vercel
vercel login
vercel deploy --prod
```

需要的环境变量：
- DATABASE_URL
- NEXTAUTH_SECRET
- STRIPE_SECRET_KEY
- 等

### 数据库
- Vercel Postgres
- Supabase
- Neon
- PlanetScale

### 文件存储
- Vercel Blob
- Cloudflare R2
- AWS S3

---

## 上线检查清单

### 产品
- [ ] 核心功能可用
- [ ] 错误处理完善
- [ ] 空状态设计
- [ ] Loading 状态
- [ ] 移动端可用

### 技术
- [ ] HTTPS
- [ ] TypeScript 严格模式
- [ ] 数据库迁移
- [ ] 环境变量管理
- [ ] CI/CD 自动部署
- [ ] 错误监控
- [ ] 性能监控

### 商业
- [ ] 隐私政策
- [ ] 服务条款
- [ ] Cookie 政策
- [ ] 退款政策
- [ ] 价格透明

### 增长
- [ ] SEO 优化
- [ ] Open Graph
- [ ] Sitemap
- [ ] 分析工具
- [ ] 邮件订阅

---

## 变现模型

### Freemium（最常见）
- 免费层：基本功能
- 付费层：高级功能 + 无限用量

### 定价建议
- 个人：$9/月
- 团队：$29/月
- 企业：$99/月

### 转化率
- 访客 → 注册：2-5%
- 注册 → 付费：1-5%
- 综合：0.05-0.25%

### 1000 MRR 需要多少流量？
- 1000 MRR @ $10 用户 = 100 付费用户
- 100 / 0.02 = 5000 注册用户
- 5000 / 0.05 = 100000 访客/月
- 约 3300 访客/天

---

## 工具栈

### 必须
- **代码**：VSCode/Cursor + GitHub
- **部署**：Vercel + PlanetScale/Supabase
- **支付**：Stripe
- **邮件**：Resend
- **错误**：Sentry
- **分析**：Plausible / PostHog

### 推荐
- **设计**：Figma
- **客服**：Crisp / HelpScout
- **邮件营销**：ConvertKit
- **SEO**：Ahrefs
- **A/B 测试**：PostHog

---

## 推荐资源

### 启动模板
- **create-t3-app**：https://create.t3.gg
- **next-saas-starter**：https://github.com/michaelfromyev/next-saas-starter

### 必读
- 《SaaS Playbook》
- 《From Impossible to Inevitable》
- https://levels.io

### 必订阅
- Lenny's Newsletter
- Demand Curve
- IndieHackers

