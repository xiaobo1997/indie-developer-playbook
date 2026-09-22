# 微信小程序开发指南

## 适合谁

- 想触达 13 亿微信用户
- 不需要复杂的原生功能
- 想快速验证想法
- 国内市场为主

## 准备工作

### 1. 注册账号
- 访问 https://mp.weixin.qq.com
- 选择"小程序"账号类型
- 主体类型：
  - **个人**：1-3 天，0 成本
  - **企业（个体工商户/公司）**：1-7 天，需营业执照

### 2. 获取 AppID
- 注册后立即获得
- 每个小程序独立

### 3. 必备工具
- **微信开发者工具**：https://developers.weixin.qq.com/miniprogram/dev/devtools/download.html
- macOS / Windows 都有

---

## 项目结构

```
project/
├── miniprogram/         # 小程序代码
│   ├── app.js          # 应用入口
│   ├── app.json        # 全局配置
│   ├── app.wxss        # 全局样式
│   ├── pages/          # 页面
│   ├── components/     # 组件
│   ├── utils/          # 工具函数
│   └── services/       # 服务层
└── cloudfunctions/     # 云函数（可选）
```

---

## 技术栈选择

### 路径 A：原生小程序
- 优点：上手快、文档全
- 缺点：未来做 App 要重写 UI

### 路径 B：Taro（推荐）
- 一套代码 → 小程序 + H5 + React Native App
- React 风格
- https://taro-docs.jd.com/

### 路径 C：uni-app
- 类似 Taro，Vue 风格
- https://uniapp.dcloud.io/

---

## 核心概念

### 1. App / Page / Component
```js
// app.js
App({
  onLaunch() { /* 启动 */ },
  globalData: {}
});

// page.js
Page({
  data: {},
  onLoad() {},
  onShow() {},
  methods: {}
});

// component.js
Component({
  properties: {},
  data: {},
  methods: {}
});
```

### 2. 数据绑定
```xml
<!-- wxml -->
<view>{{message}}</view>
<button bindtap="onTap">点击</button>
```

### 3. 列表渲染
```xml
<view wx:for="{{list}}" wx:key="id">
  {{item.name}}
</view>
```

### 4. 条件渲染
```xml
<view wx:if="{{isAdmin}}">管理员</view>
<view wx:else>用户</view>
```

---

## 后端选择

### 选项 A：微信云开发（推荐起步）
- **优势**：
  - 免备案、免域名
  - 免运维
  - 自动 HTTPS
- **功能**：
  - 云函数（Node.js）
  - 云数据库（MongoDB-like）
  - 云存储
  - 云调用（直接调微信开放接口）

### 选项 B：自建后端
- 服务器（云开发 > 自己买）
- 域名 + ICP 备案
- HTTPS 证书

---

## 关键能力

### 1. 鉴权（获取 openid）
```js
// app.js
App({
  onLaunch() {
    if (wx.cloud) {
      wx.cloud.init({
        env: 'your-env-id',
        traceUser: true
      });
    }
  }
});

// 云函数
exports.main = async (event) => {
  const { OPENID } = cloud.getWXContext();
  return { openid: OPENID };
};
```

### 2. 调用云函数
```js
const res = await wx.cloud.callFunction({
  name: 'functionName',
  data: { action: 'doSomething' }
});
```

### 3. 数据库操作
```js
const db = wx.cloud.database();

// 增
await db.collection('todos').add({
  data: { title: '学习', done: false }
});

// 查
const res = await db.collection('todos').where({
  done: false
}).get();

// 改
await db.collection('todos').doc(id).update({
  data: { done: true }
});

// 删
await db.collection('todos').doc(id).remove();
```

### 4. 文件上传
```js
const res = await wx.cloud.uploadFile({
  cloudPath: 'images/' + Date.now() + '.jpg',
  filePath: tempFilePath
});
```

### 5. 支付（需企业主体）
```js
wx.cloud.callFunction({
  name: 'pay',
  data: { orderId: 'xxx', amount: 990 }
});
```

---

## 上线流程

### 1. 完善小程序信息
- 名称、头像、简介
- 类目（个人最多 5 个）
- 服务类目

### 2. 备案（2024 起强制）
- 进入 mp.weixin.qq.com → 备案
- 提交资料，等待审核（3-7 天）
- 备案号要放在小程序底部

### 3. 提交审核
- 微信开发者工具 → 上传代码
- mp.weixin.qq.com → 版本管理 → 提交审核
- 审核时间：1-3 天

### 4. 发布
- 审核通过后点"发布"
- 用户可以搜索到

---

## 优化建议

### 性能
- 减少 setData 调用
- 图片懒加载
- 分包加载
- 减少网络请求

### 用户体验
- 启动速度 < 2s
- 加载状态明确
- 错误友好提示

### SEO
- 小程序名称含关键词
- 简介含关键词
- 标签优化

---

## 限制

### 个人主体
- ❌ 微信支付（变现路径见 [phases/08-iterate](../../../phases/08-iterate/README.md)）
- ✅ 流量主广告：个人主体可开通，条件是累计独立访客（UV）达到平台门槛（以后台要求为准）——这是个人小程序唯一的原生变现通道
- ❌ 部分类目
- ❌ 客服消息受限

### 技术限制
- 包大小：主包有上限（约 2MB），启用分包后整体上限更高（具体数值以[官方文档](https://developers.weixin.qq.com/miniprogram/dev/framework/subpackages.html)为准）
- 大资源（图片/音频/词库）建议放云存储，运行时拉取
- 不能 require 项目外文件
- 异步 API 限制

---

## 常见坑

### 1. 文件路径
- 微信**不允许** require 项目根目录外的文件
- 解决：把核心代码放在项目内

### 2. 云开发环境
- 必须先开通云开发才能用云函数
- 环境 ID 错误会报"没有权限"

### 3. 类目审核
- 教育类需要相关资质
- 工具类较宽松

### 4. 备案
- 2024 年起强制
- 没备案不能上架

---

## 资源

### 官方文档
- https://developers.weixin.qq.com/miniprogram/dev/

### 推荐库
- **Taro**：跨端框架
- **Vant Weapp**：UI 组件库
- **miniprogram-simulate**：测试

### 推荐教程
- 微信小程序官方教程
- 慕课网/极客时间小程序课程

