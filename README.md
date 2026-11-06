# LibreTV-App 修改版 Modified LibreTV-App

基于 KeyRotate/LibreTV-App（Tauri 2）修改的版本。Based on KeyRotate/LibreTV-App.

**本版本不含任何成人源。This version does NOT include any adult sources.**

## 改了哪里 / Changelog

以下所有改动均已对照原仓库（KeyRotate/LibreTV-App，commit e4078d9）逐项核对过。

### 1. 窗口：可以自由调节大小了 / Window: freely resizable

文件：`src-tauri/tauri.conf.json`

| 项目 | 原来 | 现在 |
|---|---|---|
| width | 800 | 1280 |
| height | 600 | 800 |
| resizable | true | true（不变） |
| fullscreen | false | false（不变） |
| decorations | false | true |
| maximized | true | false |

原来启动就是最大化、无边框的“假全屏”，没有标题栏、最小化、关闭按钮；现在是 1280×800 的普通窗口，有标题栏和系统按钮，可以拖动、拉边框改大小。播放器里按 F 键全屏、DPlayer 全屏功能没有动，Android 端的全屏代码没有动。

### 2. 成人源：已全部去掉 / Adult sources: removed

文件：`public/js/config.js`、`js/config.js`

- 原仓库里被注释掉的 8 个成人源（CK资源、jkun资源、百万资源、souav资源、155资源、lsb资源、黄色仓库、玉兔资源），在上一个带成人源的本地构建里曾被取消注释、内置进来；**本版本已把这 8 个全部删除**，不是重新注释，文件里不再出现它们的地址。已用 grep 全库验证：8 个成人域名 0 命中。
- 20 个普通源一个没动：电影天堂资源、如意资源、暴风资源、天涯资源、小猫咪资源、非凡影视、黑木耳、360资源、卧龙资源、华为吧资源、极速资源、豆瓣资源、魔爪资源、魔都资源、最大资源、樱花资源、百度云资源、无尽资源、旺旺短剧、iKun资源。
- 另外保留了原仓库自带的空内容测试源（地址是 example.com，不是成人源），以及原仓库 UI 里“黄色资源采集站”那组界面的通用代码——因为没有成人源数据，那组界面不会显示出任何成人源。

### 3. 成人过滤默认：恢复原值 / Adult filter default: restored

文件：`public/js/app_init.js`、`public/js/app_lifecycle.js`、`js/app.js`（各 1 行，首次初始化时的默认值）

| 项目 | 原仓库 | 带成人源的本地构建 | 本版本 |
|---|---|---|---|
| `yellowFilterEnabled` | `'true'` | `'false'` | `'true'`（恢复原值） |

也就是说，黄色内容过滤默认是开着的，和原仓库一致。

### 4. 界面：去霓虹化 / UI: de-neon theme

文件：`public/css/styles.css`、`css/styles.css`（两份内容相同，同步改了）

| 变量 / 样式 | 原来 | 现在 |
|---|---|---|
| `--primary-color` | `#00ccff`（霓虹蓝） | `#9aa8b8`（柔和灰蓝） |
| `--primary-light` | `#33d6ff` | `#b6c2cf` |
| `--secondary-color` | `#0f1622` | `#16181a` |
| `--accent-color` | `#ff3c78`（霓虹粉） | `#9aa8b8` |
| `--text-color` | `#e6f2ff` | `#e8e8e8` |
| `--text-muted` | `#8599b2` | `#9aa0a6` |
| `--border-color` | `rgba(0, 204, 255, 0.15)` | `rgba(154, 168, 184, 0.15)` |
| `--page-gradient-start` | `#0f1622` | `#16181a` |
| `--page-gradient-end` | `#192231` | `#1e2124` |
| `--card-gradient-start` | `#121b29` | `#1c1f22` |
| `--card-gradient-end` | `#1c2939` | `#22262a` |
| `--card-accent` | `rgba(0, 204, 255, 0.12)` | `rgba(154, 168, 184, 0.12)` |
| `--card-hover-border` | `rgba(0, 204, 255, 0.5)` | `rgba(154, 168, 184, 0.5)` |
| `.page-bg` 点状背景 | 霓虹蓝 / 粉 / 青三色圆点 | 统一改为灰蓝色圆点 |
| `.gradient-text` | 蓝粉渐变文字 | 纯色 `#9aa8b8` |
| `.progress-filled` | 渐变 `linear-gradient(to right, #00ccff, #3b82f6)` | 纯色 `#9aa8b8` |
| 弹窗 / 剧集按钮 / 开关阴影 | `rgba(0, 204, 255, …)` | `rgba(154, 168, 184, …)` |
| 分组标题颜色 | `#8599b2` | `#9aa0a6` |

其他相关改动（同一次去霓虹）：

- `js/player.js`、`public/js/player_dplayer_handler.js`：DPlayer 播放器主题色 `theme: '#00ccff'` → `'#9aa8b8'`
- `player.html`、`public/player.html`：左上角 logo 图标颜色 `text-[#00ccff]` → `text-[#9aa8b8]`
- `public/css/player.css`：DPlayer 设置项选中色 fallback `#00ccff` → `#9aa8b8`

以上就是全部改动，没有改其他功能。`src-tauri/tauri.conf.json` 里 `capabilities` 一项只有换行格式变化，内容仍是 `default`。

## 构建方法 / Build

```bash
npm install
npx tauri build
```

Windows 构建见原仓库 `BUILDING.md`（需安装 Rust、Tauri 依赖；交叉编译 Windows GNU 目标见该文档）。

## 使用方法 / Usage

1. 构建完成后双击运行，需要 Microsoft Edge WebView2 运行环境，Windows 11 一般自带。
2. 搜片前先在设置 → 源管理里勾选想用的源。
3. 本仓库不含任何预编译的 exe，之前那个带成人源的本地 exe **不在这个仓库里**，也不会上传到这里。

## 注意事项 / Notes

- 这些采集站都是第三方源，经常换域名、挂掉，某个源搜不出东西就换下一个用，不用重装。
- 原项目协议见 `LICENSE`（Apache 2.0），请保留原作者信息。
- 本项目不存储、不制作任何视频内容，仅聚合第三方公开接口，内容的合法性由数据源负责。
