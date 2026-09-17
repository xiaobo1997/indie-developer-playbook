# 微信小游戏开发指南

## 适合谁

- 想做休闲游戏
- 触达微信用户
- 喜欢轻量级开发
- 想做内购变现

## 小游戏 vs 小程序

| 维度 | 小程序 | 小游戏 |
|---|---|---|
| 入口 | 微信聊天 | 微信游戏中心 |
| 用户预期 | 工具 | 娱乐 |
| 开发框架 | WXML/WXSS | Canvas/WebGL |
| 性能要求 | 低 | 高 |
| 变现 | 主要广告 | 内购 + 广告 |
| 包大小 | 4MB | 8MB（首包）|

---

## 技术栈

### 路径 A：原生小游戏
- 使用微信提供的 wx API
- Canvas 2D 渲染
- 简单游戏

### 路径 B：Cocos Creator（推荐）
- 主流小游戏引擎
- 完整 IDE
- 文档完善
- https://www.cocos.com/

### 路径 C：LayaAir
- 国产引擎
- 性能好

### 路径 D：Egret
- 老牌

---

## Cocos Creator 快速开始

### 1. 安装
- 下载 Cocos Dashboard
- 安装 Cocos Creator

### 2. 创建项目
- 选择"小游戏"模板
- 选择 2D / 3D

### 3. 项目结构
```
assets/
├── scenes/        # 场景
├── scripts/       # TypeScript
├── prefabs/       # 预制体
├── textures/      # 图片
├── audio/         # 音频
└── animations/    # 动画
```

### 4. 编写脚本（TypeScript）
```typescript
import { _decorator, Component, Node } from 'cc';
const { ccclass, property } = _decorator;

@ccclass('Game')
export class Game extends Component {
  start() {
    // 初始化
  }
  
  update(deltaTime: number) {
    // 每帧更新
  }
}
```

---

## 核心功能

### 1. 用户登录
```typescript
wx.login({
  success: (res) => {
    // 发送 res.code 到服务器换 openid
  }
});
```

### 2. 用户信息
```typescript
wx.getUserProfile({
  desc: '用于显示头像和昵称',
  success: (res) => {
    console.log(res.userInfo);
  }
});
```

### 3. 分享
```typescript
wx.shareAppMessage({
  title: '一起来玩 XXX',
  imageUrl: 'share.png',
});
```

### 4. 广告变现
```typescript
// 激励视频广告
let videoAd = wx.createRewardedVideoAd({
  adUnitId: 'adunit-xxx'
});

videoAd.onLoad(() => console.log('loaded'));
videoAd.onError((err) => console.log(err));
videoAd.onClose((res) => {
  if (res.isEnded) {
    // 给奖励
  }
});

videoAd.load().then(() => videoAd.show());
```

### 5. 内购
```typescript
wx.requestMidasPayment({
  mode: 'game',
  env: 1, // 1 正式环境
  offerId: 'xxx', // 米大师后台配置
  buyQuantity: 100,
  success: (res) => {
    // 支付成功
  }
});
```

---

## 上架流程

### 1. 注册小游戏账号
- mp.weixin.qq.com → 注册 → 小游戏

### 2. 完善信息
- 名称、图标、简介
- 类目（游戏）

### 3. 接入微信开放能力
- 用户登录
- 分享
- 广告
- 内购（米大师）

### 4. 软著（必须）
- 计算机软件著作权登记
- 周期 1-3 个月
- 费用：¥500-1000

### 5. 备案
- 同小程序

### 6. 提交审核
- 1-3 天

---

## 变现

### 广告
- **激励视频**：看完给奖励，eCPM 高
- **插屏广告**：过关后弹出
- **Banner**：底部横幅

### 内购
- **米大师支付**：微信官方支付通道
- 抽成 40%（安卓）/ 30%（iOS）

### 关键指标
- **ARPU**：每用户平均收入
- **ARPDAU**：每日活跃用户平均收入
- **LTV**：用户生命周期价值
- **留存**：次日、7 日、30 日

---

## 推广

### 买量
- 微信广告（mp.weixin.qq.com/ads）
- 抖音/快手广告
- 小红书种草

### 自然流量
- 分享裂变
- 视频号内容
- 公众号关联

### 关键渠道
- 微信游戏中心
- 公众号文章
- 视频号短视频

---

## 常见坑

### 1. 包大小限制
- 首包 8MB
- 用分包加载
- 资源压缩

### 2. 性能
- iOS 性能普遍优于小游戏
- 注意低端机表现
- 避免频繁 GC

### 3. 审核
- 不能有支付方式绕过
- 用户协议要明确
- 实名认证（如涉及）

---

## 推荐学习

### Cocos 官方
- https://docs.cocos.com/

### 教程
- Cocos Creator 官方教程
- B 站搜索"Cocos 小游戏"

