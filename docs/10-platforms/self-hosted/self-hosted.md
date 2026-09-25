# 自建服务器指南

> 🚀 **想要"从购买服务器到用户用上 App"的完整按序路线**（含 K3s、中间件、CI/CD、小程序域名白名单）？看 [from-zero-to-k8s.md](from-zero-to-k8s.md)。本页是自建运维的参考手册。

## 适合谁

- 完全控制后端
- 数据隐私要求高
- 想学运维
- 长期成本可能更低

## 成本估算（月）

| 规模 | 配置 | 月成本 |
|---|---|---|
| 个人项目 | 1核 1GB | $5-10 |
| 小团队 | 2核 4GB | $20-50 |
| 中等流量 | 4核 8GB | $100-200 |
| 大流量 | 多机 + LB | $500+ |

## 推荐平台

### 海外
- **DigitalOcean**：$4/月起，简单
- **Linode**：$5/月起
- **Vultr**：$2.50/月起
- **Hetzner**：欧洲便宜
- **AWS Lightsail**：$3.50/月起

### 国内
- **阿里云 ECS**：¥40+/月
- **腾讯云 CVM**：¥40+/月
- **华为云**：类似

---

## 域名

### 域名注册商
- **Cloudflare Registrar**：成本价
- **Porkbun**：便宜
- **Namecheap**：老牌
- **腾讯云**/**阿里云**：国内（备案需要）

### 域名选择
- **.com**：通用，贵一点
- **.io**：技术感
- **.dev**：开发者
- **.cn**：国内
- **.app**：新顶级
- **.xyz**：便宜

---

## 备案（仅国内）

### ICP 备案
- 必须在 阿里云/腾讯云 办理
- 时间：7-20 工作日
- 需要：身份证、域名、服务器
- 流程：
  1. 注册账号
  2. 填写备案信息
  3. 上传资料
  4. 等待初审（1-3 天）
  5. 工信部短信核验
  6. 管局审核（7-20 天）
  7. 备案成功

### 注意事项
- 备案期间域名不能解析到服务器
- 备案号需要放在网站底部
- 个人备案不能做商业内容

---

## 必备软件

### 操作系统
- **Ubuntu LTS**（推荐）：22.04 / 24.04
- **Debian**：稳定
- **AlmaLinux / Rocky Linux**：CentOS 替代

### 基础工具
```bash
# 更新系统
sudo apt update && sudo apt upgrade -y

# 基础工具
sudo apt install -y curl wget git vim ufw fail2ban htop

# 防火墙
sudo ufw allow OpenSSH
sudo ufw allow 80
sudo ufw allow 443
sudo ufw enable

# 时间同步
sudo apt install -y chrony
```

---

## 部署方案

### 方案 A：手动部署
```bash
# 安装 Node.js (以 20.x 为例)
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt install -y nodejs

# 安装 Nginx
sudo apt install -y nginx

# 部署应用
git clone https://github.com/your/app.git /opt/app
cd /opt/app
npm ci --production
pm2 start app.js --name myapp
```

### 方案 B：Docker
```dockerfile
# Dockerfile
FROM node:20-alpine
WORKDIR /app
COPY package*.json ./
RUN npm ci --production
COPY . .
EXPOSE 3000
CMD ["node", "app.js"]
```

```yaml
# docker-compose.yml
version: '3.8'
services:
  app:
    build: .
    restart: always
    ports:
      - "3000:3000"
  db:
    image: postgres:16
    restart: always
    volumes:
      - pgdata:/var/lib/postgresql/data
    environment:
      POSTGRES_PASSWORD: ${DB_PASSWORD}
volumes:
  pgdata:
```

### 方案 C：Kubernetes（高级）
- 不推荐独立开发者
- 太复杂

---

## 反向代理 + HTTPS

### Nginx 配置
```nginx
server {
    listen 80;
    server_name example.com;
    return 301 https://$server_name$request_uri;
}

server {
    listen 443 ssl http2;
    server_name example.com;
    
    ssl_certificate /etc/letsencrypt/live/example.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/example.com/privkey.pem;
    
    location / {
        proxy_pass http://localhost:3000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

### Let's Encrypt 免费证书
```bash
sudo apt install -y certbot python3-certbot-nginx
sudo certbot --nginx -d example.com -d www.example.com
```

---

## 数据库

### PostgreSQL（推荐）
```bash
sudo apt install -y postgresql
sudo -u postgres createdb myapp
sudo -u postgres createuser myuser
```

### MySQL
```bash
sudo apt install -y mysql-server
sudo mysql_secure_installation
```

### Redis（缓存）
```bash
sudo apt install -y redis-server
sudo systemctl enable redis-server
```

### 备份
```bash
# 每日备份脚本
#!/bin/bash
BACKUP_DIR=/backup/postgres
DATE=$(date +%Y%m%d)
pg_dump myapp | gzip > $BACKUP_DIR/myapp-$DATE.sql.gz

# 上传到 S3
aws s3 cp $BACKUP_DIR/myapp-$DATE.sql.gz s3://my-backups/postgres/

# 清理 30 天前的本地备份
find $BACKUP_DIR -name "*.sql.gz" -mtime +30 -delete
```

---

## 监控

### 必备
- **UptimeRobot**：免费，监控在线
- **Sentry**：错误追踪
- **Prometheus + Grafana**：自建监控（高级）

### 推荐
- **BetterStack**：现代 Uptime
- **Netdata**：服务器资源

### 日志
- **journalctl**：Linux 系统日志
- **pm2 logs**：Node 应用日志
- **Loki + Grafana**：聚合日志

---

## 安全清单

### 上线前
- [ ] SSH 密钥登录（禁用密码）
- [ ] 防火墙开启
- [ ] fail2ban 配置
- [ ] 自动安全更新
- [ ] HTTPS 配置
- [ ] 数据库远程访问关闭
- [ ] 密钥在环境变量
- [ ] 备份策略

### SSH 密钥登录
```bash
# 本地生成密钥
ssh-keygen -t ed25519

# 上传到服务器
ssh-copy-id user@server

# 服务器端禁用密码登录
sudo sed -i 's/PasswordAuthentication yes/PasswordAuthentication no/' /etc/ssh/sshd_config
sudo systemctl restart sshd
```

### 自动安全更新
```bash
sudo apt install -y unattended-upgrades
sudo dpkg-reconfigure -plow unattended-upgrades
```

---

## 性能优化

### 数据库
- 索引
- 查询优化
- 连接池

### 应用
- 缓存（Redis）
- CDN
- 静态资源优化

### 服务器
- HTTP/2
- Gzip 压缩
- 长连接

---

## 推荐学习

### 必读
- 《Linux 命令行与 shell 脚本编程大全》
- 《鸟哥的 Linux 私房菜》

### 在线
- Linux Journey (linuxjourney.com)
- Kurose《计算机网络》

