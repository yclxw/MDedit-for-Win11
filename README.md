<h1 align="center">MD编辑器 for Win11</h1>

<p align="center">
  <strong>Windows 11 原生 Markdown 所见即所得编辑器</strong>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/版本-v3.7-0969DA?style=flat-square" alt="版本" />
  <img src="https://img.shields.io/badge/平台-Windows%2011-0078D4?style=flat-square&logo=windows" alt="平台" />
  <img src="https://img.shields.io/badge/框架-.NET%2010-512BD4?style=flat-square&logo=dotnet" alt="框架" />
  <img src="https://img.shields.io/badge/语言-C%23%2012-239120?style=flat-square&logo=csharp" alt="语言" />
  <img src="https://img.shields.io/badge/许可-Proprietary-red?style=flat-square" alt="许可" />
  <img src="https://img.shields.io/badge/架构-x64-blue?style=flat-square" alt="架构" />
</p>

<p align="center">
  <a href="#-简介">📖 简介</a> ·
  <a href="#-特性">✨ 特性</a> ·
  <a href="#-快速开始">🚀 快速开始</a> ·
  <a href="#-系统需求">💻 系统需求</a> ·
  <a href="#-技术栈">🛠 技术栈</a> ·
  <a href="#-快捷键">⌨️ 快捷键</a> ·
  <a href="#-文档">📚 文档</a> ·
  <a href="#-项目结构">📁 结构</a>
</p>

---

## 📖 简介

**MD编辑器 for Win11** 是一款面向 Windows 11 的原生 Markdown 桌面编辑器，采用 **WPF** 框架和 **WebView2 Chromium** 预览引擎构建。

> **设计哲学**：像浏览器一样管理文档。关闭窗口静默缓存全部标签页，下次打开完整恢复。仅在用户主动关闭单个标签页时才询问保存。

---

## ✨ 特性

### 核心体验
| 特性 | 说明 |
|------|------|
| 🔄 **浏览器式会话恢复** | 关闭窗口自动缓存全部标签页（含修改未保存），下次启动完整恢复至光标位置 |
| 📝 **多标签页编辑** | 无限标签页，拖放文件即打开，支持关闭/关闭其他/关闭全部 |
| 👁️ **Chromium 实时预览** | WebView2 引擎 + GitHub Primer CSS，保存后即时刷新 |
| 🎨 **GitHub Primer 主题** | 亮/暗双主题即时切换（Ctrl+Shift+D），12 个设计 Token 统一管理 |

### 编辑体验
| 特性 | 说明 |
|------|------|
| 🏷️ **7 种视图布局** | 完整视图 · 仅预览 · 仅源码 · 双栏左右 · 双栏上下 · 标准视图 · 个性视图 |
| 🧰 **32 按钮工具栏** | 48×48 ICO 四色变体图标，6 组功能分区，自动适配亮/暗主题 |
| ⌨️ **40+ 快捷键** | Ctrl+←→ 标题升降、Ctrl+↑↓ 行移动、Ctrl+Shift+1~6 标题级别 |
| 📋 **三格式剪贴板** | 复制同时含 Markdown 源码 + HTML 渲染 + 纯文本 |

### 文件与工具
| 特性 | 说明 |
|------|------|
| 📂 **文件树+大纲** | 工作区文件导航 + 标题大纲快速跳转 |
| 🔍 **查找替换** | 非模态窗口，支持正则表达式、大小写匹配 |
| 🔢 **标题重编号** | 自动添加/移除数字序号（1. / 1.1. / 2.3.1.） |
| 💾 **多格式导出** | 保存 .md / 导出 .txt / 导出 .html / 批量全部输出 |

### 工程品质
| 特性 | 说明 |
|------|------|
| 🛡️ **安全加固** | 命名管道 ACL、Mutex 单实例、JSON MaxDepth、原子写入 |
| 📦 **便携单文件** | 自包含发布（~186 MB），无需安装 .NET 运行时 |
| 🌐 **编码兼容** | BOM 自动检测（UTF-8/16/32），输出统一 UTF-8 BOM |
| ⚡ **大文件优化** | >500KB 文件异步加载，UI 线程零阻塞 |
| 💥 **三级异常保护** | Dispatcher / AppDomain / TaskScheduler 异常统一捕获 |

---

## 🚀 快速开始

### 直接运行

下载 `bak/publish/MD编辑器v3.7.20260603.exe`，双击运行。**无需安装任何运行时**。

### 从源码构建

```bash
# 前置条件：.NET 10 SDK
winget install --id Microsoft.DotNet.SDK.10 -e

# 克隆并构建
git clone <repo-url> && cd "MD编辑器 for Win11"
dotnet restore
dotnet build -c Release
dotnet run

# 发布自包含 exe
dotnet publish -c Release -r win-x64 --self-contained true \
  -p:PublishSingleFile=true -p:PublishReadyToRun=true \
  -p:IncludeNativeLibrariesForSelfExtract=true -o ./bak/publish
```

---

## 💻 系统需求

| 需求 | 最低 | 推荐 |
|------|------|------|
| 操作系统 | Windows 10 1809+ | Windows 11 |
| CPU | x64 | x64 |
| 内存 | 512 MB | 4 GB+ |
| 磁盘 | 200 MB | 500 MB |
| .NET 运行时 | 无需安装 | — |

---

## 🛠 技术栈

| 层级 | 技术 | 版本 |
|------|------|------|
| 运行时 | .NET 10 | 10.0 |
| UI 框架 | WPF | .NET 10 |
| 预览引擎 | WebView2 (Chromium) | 1.0.3124.44 |
| Markdown | Markdig | 0.34.0 |
| HTML 处理 | HtmlAgilityPack | 1.11.60 |
| 语言 | C# 12 | Nullable + ImplicitUsings |
| 架构 | Code-Behind + Service Layer | ~4,500 行 |

---

## ⌨️ 快捷键

### 文件 / 编辑
| 快捷键 | 功能 | | 快捷键 | 功能 |
|--------|------|------|--------|------|
| `Ctrl+N` | 新建 | | `Ctrl+Shift+C` | 复制为纯文本 |
| `Ctrl+O` | 打开 | | `Ctrl+F` | 查找替换 |
| `Ctrl+S` | 保存 | | `Ctrl+G` | 跳转到行 |
| `Ctrl+W` | 关闭标签 | | `Ctrl+P` | 打印 |

### 视图 / 主题
| 快捷键 | 功能 | | 快捷键 | 功能 |
|--------|------|------|--------|------|
| `Ctrl+1~7` | 7 种视图布局 | | `Ctrl+Shift+B` | 切换侧边栏 |
| `Ctrl+Shift+G` | 切换行号 | | `F11` | 全屏 |
| `Ctrl+Shift+D` | 切换亮/暗主题 | | `Ctrl+加/减/0` | 缩放 |

### 格式 / 移动
| 快捷键 | 功能 | | 快捷键 | 功能 |
|--------|------|------|--------|------|
| `Ctrl+B` / `Ctrl+I` | 粗体/斜体 | | `Ctrl+←` | 标题升级 |
| `` Ctrl+` `` | 行内代码 | | `Ctrl+→` | 标题降级 |
| `Ctrl+Shift+1~6` | H1~H6 | | `Ctrl+↑` | 行上移 |
| `Ctrl+Shift+U` | 无序列表 | | `Ctrl+↓` | 行下移 |

---

## 📚 文档

| 序号 | 文档 | 说明 |
|------|------|------|
| 01 | [系统说明](docs/01%20系统说明.md) | 系统架构、设计原则、子系统说明、开发环境与大模型 |
| 02 | [版本说明](docs/02%20版本说明.md) | 版本号规则、完整历史、兼容性矩阵、路线图 |
| 03 | [业务说明](docs/03%20业务说明.md) | 产品设计哲学、核心业务逻辑、会话管理、个性化设计 |
| 04 | [技术说明](docs/04%20技术说明.md) | 技术栈详解、架构决策、服务层 API、关键子系统 |
| 05 | [UI说明](docs/05%20UI说明.md) | 设计理念、配色方案、布局规范、工具栏/菜单/预览区 |
| 06 | [备份说明](docs/06%20备份说明.md) | 备份规范、目录结构、环境恢复、完整性验证 |
| 07 | [开发日志](docs/07%20开发日志.md) | v3.0~v3.7 完整 CHANGELOG、Keep a Changelog 规范 |

---

## 📁 项目结构

```
MD编辑器 for Win11/
├── App.xaml(.cs)                 # 应用程序入口（单实例管控、IPC）
├── MainWindow.xaml(.cs)          # 主窗口（~2,900 行）
├── FindReplaceWindow.xaml(.cs)   # 查找/替换窗口
├── MDEditor.csproj               # .NET 10 项目文件
├── Editor/MarkdownEditor         # 编辑器控件（TextBox + 行号栏）
├── Models/                       # 数据模型（Document/SessionDocument/ViewLayout）
├── Services/                     # 服务层（File/FileManager/Markdown/Settings/Clipboard）
├── Views/                        # 对话框（CustomView/GoToLine）
├── Resources/                    # 图标资源（140 个 ICO）
├── Att/                          # 应用图标
├── docs/                         # 项目文档（01~07）
└── bak/                          # 备份归档（源码/发布/图标源文件）
```

---

## 🔧 开发

| 工具 | 用途 |
|------|------|
| VS Code | IDE |
| .NET SDK 10.0 | 编译构建 |
| Claude Code | AI 辅助开发 |

| 大模型 | 厂商 | 用途 |
|--------|------|------|
| Claude Opus 4.8 | Anthropic | 架构设计、代码审查 |
| DeepSeek V4 Pro | DeepSeek | 代码生成、重构 |
| Claude Sonnet 4.6 | Anthropic | 文档编写 |
| Claude Haiku 4.5 | Anthropic | 快速补全 |

---

## 📊 统计

| 指标 | 值 | | 指标 | 值 |
|------|-----|------|------|-----|
| 代码行数 | ~4,500 | | 服务类 | 5 个 |
| C# 源文件 | 12 个 | | 快捷键 | 40+ 组 |
| XAML 文件 | 4 个 | | 已修复缺陷 | 38+ 项 |
| NuGet 依赖 | 3 个 | | 发布版本 | 7 个 |

---

## 👤 作者

**帅气的锅巴**

---

<p align="center">
  <sub>Built with .NET 10 + WPF + WebView2</sub>
</p>
