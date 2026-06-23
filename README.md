# MD文本编辑器

> 一款面向 Windows 的 Markdown 所见即所得桌面编辑器 —— 便携、离线、Chrome 级预览。

[![.NET](https://img.shields.io/badge/.NET-10-512BD4?logo=dotnet)](https://dotnet.microsoft.com/)
[![平台](https://img.shields.io/badge/Windows-10%2F11-0078D6?logo=windows)](https://www.microsoft.com/windows)
[![最新版本](https://img.shields.io/badge/版本-v3.15.20260623-2ea44f)](./docs/02%20版本说明.md)
[![许可](https://img.shields.io/badge/许可-MIT-2ea44f?logo=opensourceinitiative)](LICENSE)

---

## ✨ 功能特性

| 类别 | 特性 |
|------|------|
| **📝 编辑** | Markdown 语法高亮 · 行号显示 · GFM 扩展（表格/任务列表/数学公式） |
| **👁️ 预览** | Chromium 内核（WebView2）实时渲染 · 离线 Mermaid 图表 · 全量/快速双路径刷新 |
| **🔍 缩放** | Ctrl+滚轮缩放 · 仅预览模式缩放 WebView（0.2x~5.0x）· 状态栏实时百分比 |
| **🎨 主题** | 深色/浅色一键切换 · 15 套 CSS 预览主题（嵌入式资源） |
| **📑 多标签** | 编辑器/预览标签双向同步 · 关闭保护 · 拖放打开 · 去重逻辑 |
| **🗂️ 视图** | 7 种预设布局（仅源码/仅预览/双栏左右上下/完整/标准/自定义） |
| **💾 会话** | 退出自动保存所有标签 · 启动恢复 · 光标位置保留 |
| **📦 便携** | 单文件 exe（~170MB）· 零安装零配置 · U 盘即用 · 完全离线 |
| **⚡ 快捷键** | `Ctrl+N/O/S/W` · `Ctrl+B/I` · `Ctrl+F/H` · `Ctrl+=/-/0` 等 |
| **🖨️ 输出** | 打印预览（WebView2 `window.print()`）· 导出 HTML |

---

## 🚀 快速开始

### Windows 10 / 11

从 [Releases](../../releases) 下载最新 `MD编辑器v3.15.*.exe`，双击运行。无需安装 .NET 运行时，单文件自包含。

> 历史版本备份位于 `bak/` 目录。

### 从源码构建

```bash
# 前置要求：.NET 10.0 SDK
dotnet publish -c Release -r win-x64 --self-contained true \
  -p:PublishSingleFile=true -p:IncludeNativeLibrariesForSelfExtract=true
```

输出位于 `bin/win-x64/publish/MD编辑器v*.exe`。

---

## 📦 技术栈

| 类别 | 方案 |
|------|------|
| 运行时 | .NET 10.0 （`net10.0-windows`） |
| UI 框架 | WPF（Code-Behind 模式） |
| Markdown 解析 | [Markdig](https://github.com/xoofx/markdig) 0.34.0 |
| HTML 解析 | [HtmlAgilityPack](https://html-agility-pack.net/) 1.11.60 |
| 预览引擎 | [WebView2](https://developer.microsoft.com/microsoft-edge/webview2/)（Chromium） |
| Mermaid 图表 | `mermaid.min.js` 11.x（嵌入式资源 → 虚拟主机映射） |
| 序列化 | System.Text.Json |
| 开发工具 | VS Code + C# Dev Kit |

---

## 📁 项目结构

```
MD文本编辑器/
├── App.xaml(.cs)                # 应用入口、单实例互斥、全局异常处理
├── MainWindow.xaml(.cs)         # 主窗口（~2950 行）
├── Editor/
│   └── MarkdownEditor.xaml(.cs) # 编辑器控件（语法高亮、行号、缩放）
├── Models/                      # 数据模型
│   ├── Document.cs              # 文档实体
│   ├── ViewLayout.cs            # 7 种视图布局
│   └── SessionDocument.cs       # 会话恢复
├── Services/                    # 服务层
│   ├── MarkdownConverter.cs     # Markdown→HTML（双路径管线）
│   ├── FileManager.cs           # 文件 I/O（编码检测、大文件异步）
│   ├── SettingsService.cs       # JSON 配置管理
│   └── ClipboardService.cs      # 剪贴板工具
├── Views/                       # 子窗口
│   ├── CustomViewDialog.cs      # 自定义视图比例
│   └── GoToLineDialog.cs        # 跳转到行
├── FindReplaceWindow.xaml(.cs)  # 查找替换
├── Att/                         # 嵌入式资源
│   ├── *.css / *.md             # 15 套 CSS 预览主题
│   └── mermaid.min.js           # Mermaid 引擎（3.3MB）
├── Resources/                   # 工具栏图标（128 个 .ico）
├── PlatformHelper.cs            # 跨平台 API 适配层
├── docs/                        # 项目文档
└── bak/                         # 发布产物与源码备份
```

---

## 📚 文档索引

| 文档 | 内容 |
|------|------|
| [系统说明](docs/01%20系统说明.md) | 架构、技术栈、开发环境 |
| [版本说明](docs/02%20版本说明.md) | 版本历史、发布清单 |
| [业务说明](docs/03%20业务说明.md) | 功能特性、用户场景 |
| [技术说明](docs/04%20技术说明.md) | 技术选型、实现细节 |
| [UI 说明](docs/05%20UI说明.md) | 界面设计、颜色系统 |
| [备份说明](docs/06%20备份说明.md) | 备份策略、目录结构 |
| [开发日志](docs/07%20开发日志.md) | 变更记录 |
| [项目总结](docs/项目总结-MD编辑器v3.md) | 全面项目复盘 |
| [项目评估](docs/项目评估-MD编辑器v3.md) | 下一阶段迭代规划 |
| [五维图解（Markdown）](docs/MD编辑器-五维图解.md) | 架构/流程/数据流/模块/部署图 |
| [五维图解（HTML）](docs/MD编辑器-图解报告.html) | 图解报告（浏览器打开，含深色模式） |

---

## 🔧 开发环境

| 工具 | 版本 |
|------|------|
| 操作系统 | Windows 11 x64 |
| SDK | .NET 10.0 SDK（10.0.301） |
| 编辑器 | Visual Studio Code + C# Dev Kit |
| AI 辅助 | opencode CLI + deepseek-v4-pro |

---

## 📜 许可

[MIT](LICENSE) — 版权所有 (c) 2026 帅气的锅巴。

开放源代码，可自由使用、修改、分发，需保留版权声明。详见 [LICENSE](LICENSE) 文件。
