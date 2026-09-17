# Android App 上架指南

## 适合谁

- 想触达全球 Android 用户（25 亿+）
- 国内市场需要多个商店
- 接受较长审核周期
- 想做付费或订阅

## 前期准备

### 1. Google Play 开发者账号
- **价格**：$25（一次性）
- **注册**：https://play.google.com/console
- 需要信用卡

### 2. 国内商店（可选）
- **华为应用市场**：https://developer.huawei.com
- **小米应用商店**：https://dev.mi.com
- **OPPO 软件商店**：https://open.oppomobile.com
- **vivo 应用商店**：https://dev.vivo.com.cn
- **腾讯应用宝**：https://wiki.open.qq.com
- **百度手机助手**：http://dev.baidu.com
- 每个都需要单独注册开发者账号

### 3. 工具
- **Android Studio**（必须）
- **JDK 17+**
- **Android SDK**

### 4. 技术栈

#### 原生
- **Kotlin**（推荐）
- **Java**（老项目）
- **Jetpack Compose**（现代 UI）

#### 跨平台
- **Flutter**：Google 出品，跨 iOS/Android
- **React Native**：JS 生态
- **Taro**：跨端
- **uni-app x**

---

## 项目结构

```
android-app/
├── app/
│   ├── src/main/java/com/example/app/
│   │   ├── MainActivity.kt
│   │   ├── data/
│   │   ├── ui/
│   │   ├── domain/
│   │   └── di/
│   ├── src/main/res/
│   ├── src/main/AndroidManifest.xml
│   └── src/test/
├── build.gradle.kts
└── settings.gradle.kts
```

---

## 核心代码

### MainActivity（Jetpack Compose）
```kotlin
class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            MyAppTheme {
                Surface(modifier = Modifier.fillMaxSize()) {
                    HomeScreen()
                }
            }
        }
    }
}

@Composable
fun HomeScreen() {
    var count by remember { mutableStateOf(0) }
    Column {
        Text("Count: $count")
        Button(onClick = { count++ }) {
            Text("Increment")
        }
    }
}
```

### 网络请求（Retrofit）
```kotlin
interface ApiService {
    @GET("users/{id}")
    suspend fun getUser(@Path("id") id: String): User
}

val retrofit = Retrofit.Builder()
    .baseUrl("https://api.example.com/")
    .addConverterFactory(GsonConverterFactory.create())
    .build()
val api = retrofit.create(ApiService::class.java)
```

### 数据持久化（Room）
```kotlin
@Entity
data class User(@PrimaryKey val id: String, val name: String)

@Dao
interface UserDao {
    @Query("SELECT * FROM User")
    suspend fun getAll(): List<User>
}
```

---

## 上架流程

### Google Play

#### 1. 创建应用
- Google Play Console → Create app
- 选择默认语言、是否免费、是否含广告

#### 2. Store Listing
- 应用名称
- 简短说明（80 字）
- 完整说明（4000 字）
- 应用图标（512×512）
- Feature Graphic（1024×500）
- 截图（至少 2 张，最多 8 张）

#### 3. 内容分级
- 填写问卷
- 自动计算分级

#### 4. 隐私
- 隐私政策 URL
- 数据收集说明

#### 5. 目标受众
- 年龄分组
- 是否针对儿童

#### 6. 上传 AAB
- Build → Generate Signed Bundle
- 上传 AAB 文件

#### 7. 发布
- 选择轨道（内部测试、封闭测试、开放测试、生产）
- 提交审核
- 通常几小时到几天

### 国内商店

每个商店流程类似：
1. 注册开发者账号
2. 实名认证（个人需身份证，企业需营业执照）
3. 创建应用
4. 上传 APK/AAB
5. 填写应用信息
6. 提交审核
7. 等待 1-7 天
8. 上架

#### 国内商店特点
- **华为**：相对严格，但流量大
- **小米**：流程标准
- **OPPO/vivo**：流量大
- **应用宝**：微信生态相关
- **百度**：审核较松

---

## 支付

### Google Play 结算
- 抽成 15%（年消费 $1M 以下）
- 必须用 Google Play Billing
- 不能绕过

### 国内
- **支付宝/微信支付**：标准方案
- **银联**：大额首选
- **运营商支付**：小游戏常用

### 订阅实现
- 用 Google Play Billing Library
- 服务端验证订阅状态
- 处理续费、过期、退款

---

## 推广

### Google Play ASO
- 关键词优化
- 截图设计
- A/B 测试（Play Console 提供）

### 国内商店
- 申请"首发"、"精品推荐"
- 参加商店活动
- 投放应用商店广告

### 推广渠道
- 应用商店搜索优化
- 知乎、贴吧、微博
- 应用推荐网站（最美应用、NGA 等）

---

## 成本

### 必须
- **Google Play**：$25（一次性）
- **开发者电脑**：已有
- **服务器**：$5-100/月

### 可选
- 国内商店：免费（但流程多）
- 推广：$0-1000/月

---

## 与 iOS 开发的对比

| 维度 | iOS | Android |
|---|---|---|
| 审核 | 严格 | 宽松 |
| 上架时间 | 1-3 天 | 几小时到几天 |
| 抽成 | 30%（小开发者 15%）| 15% |
| 设备碎片化 | 少 | 多 |
| 开发工具 | 仅 macOS | macOS/Win/Linux |
| 编程语言 | Swift | Kotlin |
| 跨平台框架 | RN/Flutter/Taro | RN/Flutter/Taro |

---

## 推荐学习

### Kotlin
- Kotlin 官方文档
- Android Kotlin Fundamentals
- Google Codelabs

### Jetpack Compose
- Compose 官方教程
- Compose Pathway

### 推荐书
- 《Kotlin 实战》
- 《Android 编程权威指南》

