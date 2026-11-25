# 鱼鱼清单 🐟

一个装在手机里的待办清单：每天 / 每周 / 每月自动重复的事、临时的事、一件分成好几步的事，
都能记；勾一步涨一点进度，快到期没做完会提醒；还能一键导出到系统日历，让手机自己的闹钟响。

---

## 两种用法

### 1. 网页版（最快）

直接用手机浏览器打开 `index.html` 就能用，不用装任何东西。

详见 [使用说明.md](使用说明.md)。

### 2. 装成 App（.apk）

这个仓库里已经带好了完整的安卓工程。上传到 GitHub 会自动编译出 apk。

步骤看 [怎么变成APK.md](怎么变成APK.md)。

---

## 功能

| 你要的 | 在哪 |
|---|---|
| 每天 / 每周 / 每月 | 新建时选「每天 / 每周 / 每月」，每周可多选，每月可选几号或「月末」 |
| 临时任务 | 选「一次性」，挑个日期 |
| 自由分步骤 | 「分成几步完成」，加一步加一步 |
| 完成到哪一步 | 点一下勾上，进度条和百分比实时涨 |
| 快结束没做完提醒 | 每件事单独设「开始前提醒」「快截止再提醒」，没做完才响 |
| 连闹钟 / 日历 | 一键导出 `.ics`，导入系统日历，到点系统自己响 |

---

## 数据

- 网页版：存在浏览器 `localStorage` 里
- App 版：存在 App 自己的存储里
- 都不上传任何服务器，也没有申请任何权限
- 设置页可以导出 / 恢复 json 备份

---

## 工程结构

```
index.html                     网页版 App 本体（同时也是 apk 里的页面）
使用说明.md                     怎么用
怎么变成APK.md                  怎么编译出 apk
settings.gradle                 Gradle 项目配置
build.gradle                    AGP 版本
gradle.properties
app/build.gradle                把根目录 index.html 打进 assets
app/src/main/AndroidManifest.xml
app/src/main/java/com/yuyu/todo/MainActivity.java   WebView 壳 + .ics 导出
app/src/main/res/...            图标、主题、字符串
.github/workflows/build-apk.yml GitHub 自动编译
```
