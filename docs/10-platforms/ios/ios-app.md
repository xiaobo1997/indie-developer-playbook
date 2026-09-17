# iOS App 上架指南

## 适合谁

- 想做付费 iOS App
- 重视产品质量
- 有 6 个月以上开发周期
- 接受 Apple 的审核

## 前期准备

### 1. 苹果开发者账号（必须）
- **价格**：$99/年
- **个人账号**：简单，App 显示个人名
- **公司账号**：需要邓白氏编码（D-U-N-S）
- **注册**：https://developer.apple.com/programs/

### 2. 必备工具
- **macOS**（必须，Xcode 仅 macOS）
- **Xcode**：最新版（每年更新）
- **TestFlight**：内部测试
- **App Store Connect**：管理后台

### 3. 技术栈选择

#### 原生（性能最佳）
- Swift + SwiftUI（iOS 14+）
- Swift + UIKit（兼容老系统）

#### 跨平台
- **React Native**：JS 生态
- **Flutter**：Dart，性能好
- **Taro + Taro RN**：可输出多端
- **uni-app x**：DCloud 出品

---

## 项目结构（SwiftUI）

```
ios-app/
├── App/
│   ├── MyAppApp.swift       # 入口
│   └── ContentView.swift    # 根视图
├── Features/
│   ├── Home/
│   ├── Settings/
│   └── ...
├── Models/
├── Services/
├── UI/
│   ├── Components/
│   └── Styles/
├── Resources/
│   └── Assets.xcassets
└── Tests/
```

---

## 关键技术点

### 1. SwiftUI 入门
```swift
struct ContentView: View {
    @State private var count = 0
    
    var body: some View {
        VStack {
            Text("Count: \(count)")
            Button("Increment") {
                count += 1
            }
        }
    }
}
```

### 2. Combine 响应式
```swift
class ViewModel: ObservableObject {
    @Published var items: [Item] = []
    
    func load() async {
        items = try await api.fetch()
    }
}
```

### 3. 网络请求
```swift
// 使用 URLSession
let url = URL(string: "https://api.example.com/data")!
let (data, _) = try await URLSession.shared.data(from: url)

// 推荐：使用库
// Alamofire / Moya / URLSession 直接
```

### 4. 数据持久化
- **UserDefaults**：小数据
- **Core Data**：复杂关系数据
- **SwiftData**：现代（iOS 17+）
- **Realm**：第三方
- **CloudKit**：Apple 云

---

## 支付与订阅

### StoreKit 2（推荐）
```swift
import StoreKit

// 加载商品
let products = try await Product.products(for: ["com.app.subscription"])

// 购买
let result = try await product.purchase()

// 验证收据
guard case .success(let verification) = result else {
    return
}
```

### 订阅类型
- **自动续期订阅**：主流
- **非续期订阅**：游戏内购
- **消耗型**：游戏币

### 服务端验证
- App Store Server API
- 必须服务端验证收据（防止破解）

---

## 上架流程

### 1. 注册 App
- App Store Connect → My Apps → + → New App
- 填写：
  - 名称
  - 主要语言
  - Bundle ID（与 Xcode 一致）
  - SKU

### 2. 准备上架资料

#### 必需
- 应用截图（每种设备 1 张，可选更多）
- 应用描述
- 关键词
- 类别
- 隐私政策 URL
- 支持 URL

#### 推荐
- 应用预览视频
- 推广文本
- 版本更新说明

### 3. 提交审核

#### 必填
- 构建版本（Xcode 上传）
- 测试账号（如需要登录）
- 审核信息
  - 联系信息
  - 演示账号
  - 备注

#### 审核时长
- 一般 24-48 小时
- 节假日可能更长
- 可能被拒，需要修改后重新提交

### 4. 常见被拒原因

| 原因 | 解决 |
|---|---|
| 崩溃 | 全面测试 |
| 截图与功能不符 | 重新截图 |
| 缺少测试账号 | 提供 demo 账号 |
| 内购未完成 | 测试流程 |
| 隐私问题 | 更新隐私政策 |
| 元数据错误 | 检查所有字段 |

---

## TestFlight

### 内部测试
- 最多 100 个内部测试员
- 不需要审核
- 即时发布

### 外部测试
- 最多 10000 个外部测试员
- 需要简化的 App Store 审核
- 90 天有效期

### 使用步骤
1. Xcode → Product → Archive
2. Distribute App → TestFlight
3. 上传构建版本
4. 添加测试员
5. 测试员接受邀请
6. 下载 TestFlight App 安装

---

## 推广

### App Store Optimization (ASO)
- 标题含关键词
- 副标题
- 关键词字段
- 截图（最重要）
- 评分评论

### 上线策略
- 软启动（新西兰、加拿大）
- Product Hunt
- Reddit
- Twitter
- TechCrunch 投稿（重大更新）

### 持续运营
- 定期更新
- 节日活动
- 用户反馈响应
- 数据驱动迭代

---

## 成本

### 必须
- **Apple Developer Program**：$99/年
- **服务器**：$5-100/月
- **域名**：$10/年

### 可选
- **Analytics**：免费版够用
- **Crash Reporting**（Sentry）：免费额度
- **设计素材**：$10-100 一次性

### 时间
- 开发：1-6 个月
- 审核：1-3 天
- 上线后持续运营

---

## 推荐学习

### 官方
- Swift 官方教程
- SwiftUI 教程
- Apple 开发者文档

### 推荐课程
- Hacking with Swift（Paul Hudson）
- Design Code
- Sean Allen YouTube

### 推荐书
- 《Swift 编程权威指南》
- 《iOS 编程：The Big Nerd Ranch Guide》

