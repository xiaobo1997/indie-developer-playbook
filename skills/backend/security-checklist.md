# Skill: 安全清单

> 上线前必查的 30 项安全检查

## 认证 & 授权

- [ ] 所有 API 都需要认证（除明确公开的）
- [ ] 密码用 bcrypt 哈希（cost >= 10）
- [ ] 实现密码强度检查
- [ ] 实现登录失败限速
- [ ] Session token 过期时间合理
- [ ] 支持会话失效
- [ ] 实现 MFA（如果可能）

## 输入验证

- [ ] 所有用户输入都验证（白名单 > 黑名单）
- [ ] SQL 用参数化查询（不用字符串拼接）
- [ ] 用户输入输出到 HTML 时转义
- [ ] 文件上传验证类型和大小
- [ ] URL 重定向白名单
- [ ] JSON 反序列化验证 schema
- [ ] XML 禁用外部实体（XXE）

## 数据保护

- [ ] HTTPS 强制（包括子域名）
- [ ] HSTS 头
- [ ] 数据库密码不在代码里
- [ ] 密钥在环境变量
- [ ] 数据库连接用 SSL
- [ ] 敏感数据加密存储
- [ ] 定期备份并测试恢复

## HTTP 安全

- [ ] CSP 头（Content-Security-Policy）
- [ ] X-Frame-Options: DENY
- [ ] X-Content-Type-Options: nosniff
- [ ] Referrer-Policy: strict-origin-when-cross-origin
- [ ] CORS 配置正确
- [ ] Cookie 加 Secure, HttpOnly, SameSite

## 依赖安全

- [ ] npm audit 无高危漏洞
- [ ] 依赖定期更新（Dependabot）
- [ ] 不使用过时版本
- [ ] 锁定版本（package-lock.json）

## 监控

- [ ] 错误日志记录
- [ ] 异常登录告警
- [ ] 速率限制（rate limiting）
- [ ] API 配额
- [ ] 敏感操作审计日志

## 运营

- [ ] 事故响应计划
- [ ] 漏洞披露流程
- [ ] 定期安全审计
- [ ] 员工安全培训

## 推荐工具

- [Mozilla Observatory](https://observatory.mozilla.org/)
- [Snyk](https://snyk.io/)
- [GitHub Dependabot](https://github.com/dependabot)

