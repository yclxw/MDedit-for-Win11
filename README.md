MD 文本编辑器
=============

适用于 Windows 的桌面 Markdown 编辑工具，基于 .NET 10 + WPF。

当前版本：**v2.57.202605242121** — [VERSION.md](VERSION.md) · [CHANGELOG.md](CHANGELOG.md) · [BACKUP.md](BACKUP.md)

----

目录
====

1. [概述](#1-概述)
2. [功能特性](#2-功能特性)
3. [系统需求](#3-系统需求)
4. [快速上手](#4-快速上手)
5. [视图模式](#5-视图模式)
6. [快捷键](#6-快捷键)
7. [配置项](#7-配置项)
8. [源码结构](#8-源码结构)
9. [技术架构](#9-技术架构)
10. [构建](#10-构建)
11. [开发环境](#11-开发环境)
12. [已知问题](#12-已知问题)
13. [作者](#13-作者)

----

1. 概述
====

MD 文本编辑器（以下简称 MDEditor）是一款面向技术写作与文档编辑场景的桌面 Markdown 编辑器。提供多标签页编辑、实时 HTML 预览、七种视图布局，以及 40+ 键盘快捷键。

**适用场景:**

- 编写 README、API 文档、技术笔记
- 离线 Markdown 写作与导出
- 文件夹工作区下的批量文档浏览

----

2. 功能特性
====

文件管理
----

- 新建、打开、保存 ``.md`` / ``.txt`` 文件
- 另存为 ``.md``、``.txt``、``.html`` 三种格式
- 打开文件夹作为工作区，路径自动记忆
- 多标签页编辑，脏文档 ``*`` 标记
- 最近文件列表（最多 20 条，去重排序）
- 打印（调用系统打印对话框）

**单实例机制:**

双击 ``.md`` 文件时，若程序已运行，新文件在当前窗口以标签页打开，不重复启动。
通过命名 Mutex 检测实例，NamedPipe 跨进程传递路径。

**会话恢复:**

窗口关闭时自动保存所有标签页（路径、内容、脏状态、光标位置），
下次启动自动恢复。与命令行传入的文件同时打开，互不覆盖。

编辑功能
----

- 撤销/重做、剪切/复制/粘贴（WPF ``TextBox`` 原生支持）
- 复制为纯文本（剥离 Markdown 语法标记）
- 复制为 Markdown、复制为 HTML
- 粘贴为纯文本（剥离外部格式）
- 查找与替换（区分大小写、正则表达式），F3 查找下一个
- 跳转到指定行

格式快捷键
----

| 格式 | 快捷键 | 语法 |
|------|--------|------|
| 标题 1–6 | ``Ctrl+Shift+1`` ~ ``Ctrl+Shift+6`` | ``#`` ~ ``######`` |
| 粗体 | ``Ctrl+B`` | ``**text**`` |
| 斜体 | ``Ctrl+I`` | ``*text*`` |
| 删除线 | ``Ctrl+Shift+X`` | ``~~text~~`` |
| 行内代码 | ``Ctrl+` `` | `` `code` `` |
| 代码块 | ``Ctrl+Shift+` `` | `` ``` `` |
| 引用块 | ``Ctrl+Shift+Q`` | ``> quote`` |
| 无序列表 | ``Ctrl+Shift+U`` | ``- item`` |
| 有序列表 | ``Ctrl+Shift+N`` | ``1. item`` |
| 任务列表 | ``Ctrl+Shift+T`` | ``- [ ] task`` |
| 链接 | ``Ctrl+Shift+K`` | ``[text](url)`` |
| 图片 | ``Ctrl+Shift+I`` | ``![alt](url)`` |
| 数学公式 | — | ``$$`` 块 |
| Mermaid | — | `` ```mermaid `` 块 |

选中文本时自动包裹，无选中时在光标处插入模板。预览采用 150ms 防抖。

工具
----

- **字数统计:** 统计当前文档字数
- **TOC 生成:** 扫描标题，生成锚点目录列表
- **文件关联:** ``HKCU`` 注册表关联 ``.md``（无需管理员权限），支持取消

菜单栏指示器
----

菜单栏右侧四色切换按钮（20×20px，圆角 3px，默认不透明度 0.7，悬停 1.0）:

| 符号 | 颜色 | 名称 | 功能 |
|------|------|------|------|
| ☰ | ``#E74C3C`` | 红 | 侧边栏开关 |
| ◎ | ``#F1C40F`` | 黄 | 预览区开关 |
| ◼ | ``#3498DB`` | 蓝 | 编辑区开关 |
| # | ``#2ECC71`` | 绿 | 行号开关 |

----

3. 系统需求
====

| 需求项 | 最低要求 |
|--------|----------|
| 操作系统 | Windows 10（1809+）或 Windows 11 |
| CPU 架构 | x64 |
| 内存 | 512 MB |
| 磁盘 | 200 MB（自包含版本） |
| .NET 运行时 | .NET 10（仅框架依赖版需要） |

自包含发布版本内嵌 .NET 运行时，无需系统预装。

----

4. 快速上手
====

自包含版本
----

下载 ``MD文本编辑器v{版本号}.exe``，双击运行。无需安装依赖。

从源码运行
----

```powershell
# 安装 .NET 10 SDK
winget install --id Microsoft.DotNet.SDK.10 -e

# 获取源码
git clone <repo-url>
cd MDEditor

# 还原依赖
dotnet restore

# 编译并运行
dotnet run
```

----

5. 视图模式
====

可用视图（``Ctrl+1`` ~ ``Ctrl+7``）
----

| 序号 | 模式 | 快捷键 | 布局 |
|------|------|--------|------|
| 1 | 完整视图 | ``Ctrl+1`` | 侧边栏 + 编辑区 + 预览区三栏 |
| 2 | 仅预览 | ``Ctrl+2`` | 全屏预览 |
| 3 | 仅源码 | ``Ctrl+3`` | 全屏编辑 |
| 4 | 双栏（左右） | ``Ctrl+4`` | 编辑左 / 预览右，分割线可拖拽 |
| 5 | 双栏（上下） | ``Ctrl+5`` | 编辑上 / 预览下，分割线可拖拽 |
| 6 | 标准视图 | ``Ctrl+6`` | 编辑 1/3 + 预览 2/3 |
| 7 | 个性视图 | ``Ctrl+7`` | 自定义三栏占比（总和 100%） |

默认视图在菜单「视图 → 默认视图设置」中选择，重启生效。

其他视图功能
----

| 操作 | 快捷键 | 说明 |
|------|--------|------|
| 全屏 | ``F11`` | 窗口最大化/还原 |
| 侧边栏 | ``Ctrl+Shift+B`` | 文件树 + 大纲面板 |
| 行号 | ``Ctrl+Shift+G`` | 编辑区行号显示 |
| 深色/浅色主题 | ``Ctrl+Shift+D`` | UI + 预览区同步切换 |
| 放大/缩小/重置 | ``Ctrl+=`` / ``Ctrl+-`` / ``Ctrl+0`` | 编辑器字体缩放 |

----

6. 快捷键
====

文件
----

| 操作 | 快捷键 |
|------|--------|
| 新建 | ``Ctrl+N`` |
| 打开 | ``Ctrl+O`` |
| 保存 | ``Ctrl+S`` |
| 另存为 | ``Ctrl+Shift+S`` |
| 打印 | ``Ctrl+P`` |
| 关闭标签页 | ``Ctrl+W`` |

编辑
----

| 操作 | 快捷键 |
|------|--------|
| 复制为纯文本 | ``Ctrl+Shift+C`` |
| 粘贴为纯文本 | ``Ctrl+Shift+V`` |
| 查找/替换 | ``Ctrl+F`` |
| 查找下一个 | ``F3`` |
| 跳转到行 | ``Ctrl+G`` |

``Ctrl+Z``、``Ctrl+Y``、``Ctrl+X``、``Ctrl+C``、``Ctrl+V``、``Ctrl+A``
由 WPF ``TextBox`` 原生处理，无须代码绑定。

----

7. 配置项
====

配置文件位于 ``%LOCALAPPDATA%\MDEditor\settings.json``:

```json
{
  "IsDarkTheme": false,
  "AutoSaveEnabled": true,
  "AutoSaveIntervalMinutes": 5,
  "RecentFiles": [],
  "LastOpenedFolder": null,
  "DefaultView": "FullView",
  "CustomSidebarRatio": 20,
  "CustomEditorRatio": 40,
  "CustomPreviewRatio": 40,
  "EditorFontFamily": "Consolas",
  "EditorFontSize": 14
}
```

字段说明:

| 字段 | 类型 | 默认值 | 说明 |
|------|------|--------|------|
| ``IsDarkTheme`` | bool | false | 深色/浅色主题 |
| ``AutoSaveEnabled`` | bool | true | 启用自动保存 |
| ``AutoSaveIntervalMinutes`` | int | 5 | 自动保存间隔（分钟） |
| ``RecentFiles`` | string[] | [] | 最近文件（最多 20） |
| ``LastOpenedFolder`` | string? | null | 工作区路径，启动自动恢复 |
| ``DefaultView`` | string | FullView | 默认视图模式 |
| ``CustomSidebarRatio`` | int | 20 | 个性视图：侧边栏占比 % |
| ``CustomEditorRatio`` | int | 40 | 个性视图：编辑区占比 % |
| ``CustomPreviewRatio`` | int | 40 | 个性视图：预览区占比 % |
| ``EditorFontFamily`` | string | Consolas | 编辑区字体 |
| ``EditorFontSize`` | double | 14 | 编辑区字号 |

会话文件 ``%LOCALAPPDATA%\MDEditor\session.json`` 存储标签页状态，
启动时自动恢复。

删除 ``%LOCALAPPDATA%\MDEditor\`` 恢复全部默认设置。

----

8. 源码结构
====

```
.
├── App.xaml / App.xaml.cs         # 应用入口（单实例、管道、异常处理）
├── MainWindow.xaml / .cs          # 主窗口（UI、事件、快捷键 1955 行）
├── FindReplaceWindow.xaml / .cs   # 查找/替换对话框
├── MDEditor.csproj                # 项目文件（net10.0-windows）
├── Editor/
│   └── MarkdownEditor.xaml / .cs  # 自定义编辑器控件（397 行）
├── Models/
│   ├── Document.cs                # 文档模型
│   ├── SessionDocument.cs         # 会话持久化模型
│   └── ViewLayout.cs              # 视图布局描述结构
├── Services/
│   ├── FileManager.cs             # 文件操作服务（91 行）
│   ├── FileService.cs             # 文件 I/O 底层
│   ├── ClipboardService.cs        # 剪贴板服务
│   ├── MarkdownConverter.cs       # Markdown → HTML 渲染器（103 行）
│   └── SettingsService.cs         # 设置与会话持久化（121 行）
├── Views/
│   ├── CustomViewDialog.cs        # 个性视图配置对话框
│   └── GoToLineDialog.cs          # 跳转到行对话框
├── docs/                          # 业务逻辑文档
├── bak/                           # 备份、构建产物、工具
├── Att/logo.ico                   # 应用图标
├── CHANGELOG.md                   # 更新日志
├── VERSION.md                     # 版本信息
└── README.md                      # 本文件
```

总代码量约 3400 行（含 XAML）。

----

9. 技术架构
====

分层
----

| 层 | 职责 | 实现 |
|----|------|------|
| 应用层 | 生命周期、单实例、IPC | Mutex + NamedPipe + 全局异常处理 |
| UI 层 | 窗口、菜单、快捷键 | XAML + Code-Behind |
| 编辑器层 | 文本编辑、行号、Tab 缩进 | 自研 ``UserControl``，封装 ``TextBox`` |
| 渲染层 | Markdown → HTML | Markdig 管道 + 内嵌 CSS |
| 预览层 | HTML 展示 | WPF ``WebBrowser``（IE Trident） |
| 服务层 | I/O、剪贴板、持久化 | 静态方法类 |
| 数据层 | 文档/设置/会话 | POCO，JSON 序列化 |

关键设计决策
----

- **无 MVVM:** Code-Behind 模式，减少抽象层。快捷键通过
  ``RoutedUICommand`` + ``KeyBinding`` 注册。
- **单实例:** 命名 ``Mutex`` 检测，``NamedPipeServerStream`` 转发文件路径。
  第二实例 ``Environment.Exit(0)`` 快速退出。
- **异步大文件:** 阈值 500KB。先创建占位标签，后台 ``Task.Run()`` 读取，
  ``Dispatcher.InvokeAsync`` 回填 UI。
- **预览防抖:** 150ms ``DispatcherTimer``，内容+主题未变则跳过渲染。
- **视图状态机:** ``ViewLayout`` record struct 统一描述 7 种视图 + 自定义布局。
  色条基于 ``_currentViewLayout`` 做三态决策。

依赖
----

| 包 | 版本 | 用途 |
|----|------|------|
| `Markdig <https://github.com/xoofx/markdig>`_ | 0.34.0 | Markdown 解析与 HTML 生成 |
| `HtmlAgilityPack <https://html-agility-pack.net/>`_ | 1.11.60 | HTML → 纯文本 |

- 目标框架: ``net10.0-windows``
- WPF: UI
- Windows Forms: ``FolderBrowserDialog``

----

10. 构建
====

开发构建::

  dotnet build -c Debug

发布构建::

  dotnet build -c Release

自包含单文件发布::

  dotnet publish -c Release -r win-x64 --self-contained true \
    -p:PublishSingleFile=true \
    -p:IncludeNativeLibrariesForSelfExtract=true \
    -o ./publish

产物: ``publish/MD文本编辑器v2.57.202605242121.exe``，约 174 MB。

源码打包::

  $ver = Get-Date -Format "yyyyMMddHHmm"
  Compress-Archive -Path "*.csproj","*.xaml","*.cs","Editor","Models","Services","Views","Att","docs","*.md" \
    -DestinationPath "bak\source\MDEditor-src-$ver.zip" -Force

备份策略详见 `BACKUP.md <BACKUP.md>`_。

----

11. 开发环境
====

开发软件
----

本项目使用以下软件进行开发：

| 软件 | 版本 | 用途 |
|------|------|------|
| `Visual Studio Code <https://code.visualstudio.com/>`_ | 稳定版 | 代码编辑、项目浏览 |
| `.NET 10 SDK <https://dotnet.microsoft.com/download/dotnet/10.0>`_ | 10.0.x | 编译、构建、发布 |
| `PowerShell 5.1 / 7.x <https://learn.microsoft.com/powershell/>`_ | — | 命令行与构建脚本 |
| `Git <https://git-scm.com/>`_ | 稳定版 | 版本控制 |
| `Claude Code <https://claude.ai/code>`_ | CLI v2.x | AI 辅助开发 |

AI 大模型
----

| 模型 | 提供商 | 上下文 | 主要用途 |
|------|--------|--------|----------|
| DeepSeek V4 Pro | DeepSeek | 1M tokens | 代码分析、逻辑梳理、方案设计 |
| Claude Opus 4.7 | Anthropic | 200K tokens | 架构规划、文档撰写、代码审查 |
| Claude Sonnet 4.6 | Anthropic | 200K tokens | 代码生成、快速迭代、问题修复 |

Claude Code 通过 VS Code 扩展接入，负责本项目约 95% 的代码生成、
审查与重构工作。Prompt 复杂度从单行指令到多文件重构不等。

代码风格
----

- 命名空间: ``MDEditor`` / ``MDEditor.Editor`` / ``MDEditor.Models`` / ``MDEditor.Services``
- Null 安全: ``<Nullable>enable</Nullable>``
- XAML 资源: ``DynamicResource``（支持运行时主题切换）
- 注释: 仅对非显而易见的逻辑添加

----

12. 已知问题
====

**预览引擎**

基于 IE Trident 引擎（WPF ``WebBrowser`` 控件），不支持 ``prefers-color-scheme``、
CSS 自定义属性、``display: grid`` 等现代特性。KaTeX 与 Mermaid 代码仅以
原始文本形式显示。计划后续支持 WebView2 引擎。

**编码**

所有文件以 UTF-8 读写。不支持其他编码的自动检测。

**平台**

仅支持 Windows 10（1809+）/ Windows 11 x64。

**启动行为**

窗口最大化启动，不自动创建空白文档。若上次打开了工作区，自动恢复文件树。

** .NET 10 适配**

目标框架 ``net10.0-windows``。由于 ````System.Windows.Controls.WebBrowser````
在 .NET 10 中仍依赖 IE Trident，当系统未安装 IE 组件时（Windows 11 纯净安装）
预览功能可能不可用。

----

13. 作者
====

**帅气的锅巴** — 设计、开发、文档。

本软件为个人开发项目，保留所有权利，仅供学习与个人使用。

问题反馈请通过项目仓库提交 Issue。
