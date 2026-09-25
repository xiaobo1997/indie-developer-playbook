# 平台管线 · Web / H5

> 唯一"无审核环节"的平台：上线即完成。发布 = 部署 + 域名 + E2E 验证。
> 服务器路线全文：[from-zero-to-k8s](../../../docs/10-platforms/self-hosted/from-zero-to-k8s.md)；单机简化：[self-hosted](../../../docs/10-platforms/self-hosted/self-hosted.md)。

## 硬门槛（预检项）

| 检查项 | 要求 | 依据 |
|---|---|---|
| 代码可构建 | `npm run build`（或对应栈）成功，产出静态目录 | 实测 |
| 部署目标 | k3s / compose / vercel 三选一已在参数表 | params |
| 域名 | 已购买；**国内服务器需 ICP 备案**（7-20 工作日，最大周期门槛） | 以注册商为准 |
| 预算 | 服务器月费在 budget_cap 内 | params |

## 自动化通道（🤖 全程无浏览器）

- 部署与配置：SSH + kubectl/compose（见部署清单 A-J 组）
- DNS：云厂商 DNS API；证书：cert-manager 自动签
- CI/CD：GitHub Actions（构建→推镜像→rollout）

## 流水线

| # | 步骤 | 谁 |
|---|---|---|
| W-01 | build 成功，产物可本地起服务自测 | 🤖 |
| W-02 | 按 [checklist-zero-to-prod](../../../docs/10-platforms/self-hosted/checklist-zero-to-prod.md) 执行到 G 组（部署完成） | 🤖（👤 停机点见该清单） |
| W-03 | DNS A 记录生效 + HTTPS 证书签发 | 🤖 |
| W-04 | **E2E 验收** | 🤖 |

## E2E 验收（交付标准）

- [ ] 外部网络 `curl -I https://<域名>` → 200，证书有效
- [ ] 核心链路真实走通：注册 → 核心功能 → 退出重进数据还在
- [ ] 移动端浏览器打开无布局崩坏（至少 iPhone Safari + 一台安卓 Chrome）
- [ ] 监控告警能触达 params 里的 email

## 无审核，但有"发布检查"

上线前走一遍 [deploy-checklist](../../devops/deploy-checklist.md)；Landing Page 与增长动作用 [phases/07-launch](../../../phases/07-launch/README.md)。
