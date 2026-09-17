# 案例：Nomad List

## 基本信息

- **产品**：https://nomadlist.com
- **创始人**：Pieter Levels (@levelsio)
- **上线**：2014
- **类型**：数字游民社区 + 城市数据
- **技术栈**：单文件 PHP + jQuery + MySQL

## 产品做什么

帮助远程工作者选择居住城市：
- 全球 1000+ 城市的实时数据
- 成本（房租、餐饮、交通）
- 天气、空气质量、安全
- 网速、夜生活、社区
- 用户评价

## 收入模型

### 免费浏览
- 用户可看公开数据

### 会员订阅 ($99/年)
- 完整数据
- 高级筛选
- 社区讨论
- 私信

### 公开数据
- 收入：$50 万+/年
- 用户：10 万+
- 续订率：80%+

## 为什么成功

### 1. 解决真实问题
- Pieter 自己就是数字游民
- 亲身经历找城市的痛点
- 自己愿意付费

### 2. 数据驱动
- 公开抓取数据
- 用户提交真实体验
- 不断更新

### 3. 极简执行
- 单文件 PHP，没有"工程化"
- 部署简单
- 一个人维护

### 4. 公开构建
- 每天 Twitter 更新
- 直播编程
- 高度透明

## 复刻这个产品的关键

1. **解决自己的痛点**：你愿意每月付 $99 吗？
2. **数据丰富**：信息密度要高
3. **社区粘性**：用户评价是关键
4. **公开透明**：让用户信任

## 代码示例（简化版）

```php
<?php
// index.php - 主页面
$cities = $db->query("SELECT * FROM cities ORDER BY nomad_score DESC LIMIT 50");
?>
<!DOCTYPE html>
<html>
<head><title>Nomad List</title></head>
<body>
<h1>Best cities for digital nomads</h1>
<?php foreach ($cities as $city): ?>
  <div class="city">
    <h2><?= $city['name'] ?></h2>
    <p>$<?= $city['monthly_cost'] ?>/mo</p>
    <p>WiFi: <?= $city['wifi_speed'] ?> Mbps</p>
  </div>
<?php endforeach ?>
</body>
</html>
```

就这么多。没有框架，没有构建工具。

