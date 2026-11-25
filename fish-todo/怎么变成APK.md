# 怎么把这个变成 .apk

先说清楚：**我这边环境坏了，编译不了**（命令行进程起不来，`0xC0000142`），所以没法直接把 `app-debug.apk` 递给你。

但工程我已经全写好了，放在这个 `fish-todo` 文件夹里。你按下面走一遍，**GitHub 会免费帮你编译**，最后你下载 apk 装到手机上，十分钟的事，不用装 Android Studio。

---

## 路线 A：GitHub 自动编译（推荐，不用装任何东西）

### 1. 注册 / 登录 GitHub

打开 <https://github.com>，注册一个账号（免费）。

### 2. 建一个仓库

右上角 **+** → **New repository**

- Repository name：随便填，比如 `yuyu-todo`
- 选 **Public**（公开仓库用 Actions 完全免费）
- 下面的勾选项都别勾
- 点 **Create repository**

### 3. 把整个文件夹传上去

新仓库页面会有一个 **"uploading an existing file"** 的链接，点它。

然后把 `fish-todo` 文件夹里的**所有东西**拖进去（整个文件夹拖，或者全选拖）：

```
index.html
使用说明.md
settings.gradle
build.gradle
gradle.properties
.gitignore
app/            ← 整个目录
.github/        ← 整个目录（里面是自动编译的配置）
```

> ⚠️ **`.github` 这个目录一定要传上去**，没有它就不会自动编译。
> Windows 资源管理器里它可能显示成灰色/半透明，那只是以点开头，不是隐藏，能拖。

拖完下面点 **Commit changes**。

### 4. 手动启动一次编译

刚传上去的时候，自动编译**不会自己跑**（GitHub 的规矩：新加的配置文件要等下一次提交才生效）。

所以：

1. 点仓库顶部的 **Actions** 标签
2. 左边会看到 **build-apk**，点它
3. 右边有个 **Run workflow** 按钮，点开，再点绿色的 **Run workflow**

### 5. 等 5～10 分钟

刷新页面，会看到一个黄色转圈 → 变成绿色对勾。

第一次要下载 Gradle 和 Android 依赖，慢一点是正常的。

### 6. 下载 apk

点进那个绿色的构建记录，拉到最底下 **Artifacts** 区域：

- 点 **yuyu-todo-apk** 下载（会得到一个 zip）
- 解压，里面是 **`app-debug.apk`**
- 传到手机，点开安装

> 安装时手机会提示"未知来源""不安全"——因为这不是应用商店的包，正常，允许一次就行。

---

## 路线 B：电脑上装 Android Studio 编译

如果你电脑上已经有 Android Studio：

1. Android Studio → **Open** → 选 `fish-todo` 这个文件夹
2. 等它同步完（会提示 Gradle 版本，让它自己下）
3. 菜单 **Build** → **Build Bundle(s) / APK(s)** → **Build APK(s)**
4. 编译完右下角会弹 "locate"，点开就是 `app-debug.apk`

需要：JDK 17、Android SDK 34。

---

## 路线 C：让我再来一次

如果哪天这台机器的命令行能起来了，跟我说一声，我直接编好把 apk 给你。

---

## 工程里都有什么

| 文件 | 干什么的 |
|---|---|
| `index.html` | 就是那个 App 本体，原封不动打包进去 |
| `MainActivity.java` | 一个 WebView 壳，负责显示网页 + 处理 .ics 导出 |
| `AndroidManifest.xml` | 声明 App 名字、图标、启动页 |
| `app/build.gradle` | 把根目录的 `index.html` 拷进 assets 再打包 |
| `.github/workflows/build-apk.yml` | GitHub 自动编译的配置 |

**没有申请任何权限**（不联网、不读通讯录、不读相册）。日历是靠导出一个 `.ics` 文件交给系统日历，不是偷偷改你的日历。

---

## 可能会遇到的问题

**Actions 里没有 build-apk 这个 workflow？**
说明 `.github/workflows/build-apk.yml` 没传上去。在仓库页面 **Add file → Create new file**，
文件名填 `.github/workflows/build-apk.yml`，把那个 yml 的内容粘进去，提交后就能手动跑了。

**构建红了（红叉）？**
点进去看最后几十行报错，把红色那段截图发我，我改。

**装到手机上打开是白屏？**
APK 里用的是 https 虚拟域名加载页面，理论上不会白。真白屏的话跟我说，我换成 `file://` 加载。

**导出日历没反应？**
APK 里导出会存到 `Android/data/com.yuyu.todo/files/Documents/`，
同时弹出一个"选日历 App"的选择框。选系统日历就能导入。

---

## 装好之后

跟网页版功能完全一样：每天/每周/每月、临时任务、分步骤、进度百分比、到期提醒，
外加一个独立图标、独立存储、不依赖浏览器。
