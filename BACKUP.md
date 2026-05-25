BACKUP
======

MD 文本编辑器 — 项目备份与恢复规范

:版本: v2.57.202605242121
:日期: 2026-05-24
:路径: ``bak/``

----

目录
====

1. [概述](#1-概述)
2. [目录结构](#2-目录结构)
3. [各目录说明](#3-各目录说明)
4. [备份操作](#4-备份操作)
5. [恢复验证](#5-恢复验证)
6. [版本对应关系](#6-版本对应关系)
7. [注意事项](#7-注意事项)
8. [相关文档](#8-相关文档)

----

1. 概述
====

``bak/`` 目录为项目备份根目录，存放源码归档、发布产物、开发工具指引与运行时补丁。

备份目标:

- 源码可重建: 任意 Windows 机器上解压源码包即可编译
- 产物可直接运行: 自包含 ``.exe`` 无需安装运行时
- 环境可复现: 工具清单与安装脚本完备

----

2. 目录结构
====

::

    bak/
    ├── source/              完整源代码归档 (.zip)
    ├── dev-tools/           开发工具集 (安装指引/脚本)
    ├── patches/             运行补丁集 (WebView2/字体/注册表)
    ├── publish/             发布产物 (自包含 .exe)
    └── (本文件归档于 docs/BACKUP.md)

----

3. 各目录说明
====

3.1 source/ — 源代码归档
----

存放项目全部源代码的 ZIP 压缩包。

**包含:**

- ``*.csproj`` 项目文件
- ``*.xaml`` / ``*.cs`` 全部源文件
- ``Editor/`` ``Models/`` ``Services/`` ``Views/`` ``Att/`` ``docs/`` 等子目录
- ``VERSION.md`` ``CHANGELOG.md`` ``README.md`` 等文档

**不包含:**

- ``bin/`` ``obj/`` ``.vs/`` 编译产物
- ``bak/`` 备份目录自身（避免递归）
- ``publish/`` 发布产物（体积过大，单独存放）

:命名格式: ``MDEditor-src-YYYYMMDD.zip``
:当前文件: ``MDEditor-src-20260524.zip`` (179 KB)

3.2 dev-tools/ — 开发工具集
----

存放开发环境所需工具的安装指引与脚本。

| 工具 | 版本要求 | 用途 |
|------|----------|------|
| .NET 10 SDK | 10.0.x | 编译与发布 |
| Visual Studio Code | 最新稳定版 | 代码编辑与项目导航 |
| Git for Windows | 最新稳定版 | 版本控制 |
| Claude Code | CLI v2.x | AI 辅助开发 |
| PowerShell | 5.1 / 7.x | 命令行与构建脚本 |

> 大型安装包不直接存放，改为下载链接。运行 ``dev-tools\setup-env.ps1`` 一键配置。

3.3 patches/ — 运行补丁集
----

存放目标部署环境中可能需要的运行时补丁或配置脚本:

- WebView2 运行时安装器（可选，增强预览 CSS 支持）
- 等宽字体包（Consolas、Segoe UI Symbol）
- 注册表补丁（``.md`` 文件关联）

3.4 publish/ — 发布产物
----

存放版本化的自包含单文件发布产物。

:文件格式: ``MD文本编辑器v{版本号}.exe``
:当前文件: ``MD文本编辑器v2.57.202605242121.exe`` (174 MB)

自包含发布内嵌 .NET 10 运行时，可直接在 Windows 10 (1809+) / Windows 11 x64 上运行，
无需系统预装 .NET。

----

4. 备份操作
====

4.1 完整备份 (每次发布前执行)
----

**步骤 1 — 打包源码::

    $ver = Get-Date -Format "yyyyMMdd"
    Compress-Archive -Path @(
        "*.csproj", "*.xaml", "*.cs",
        "Editor\", "Models\", "Services\", "Views\", "Att\", "docs\",
        "VERSION.md", "CHANGELOG.md", "README.md"
    ) -DestinationPath ".\bak\source\MDEditor-src-$ver.zip" -Force

**步骤 2 — 生成发布产物::

    dotnet publish -c Release -r win-x64 --self-contained true `
      -p:PublishSingleFile=true `
      -p:IncludeNativeLibrariesForSelfExtract=true `
      -o ./publish

**步骤 3 — 版本控制提交::

    git add bak\source\*.zip publish\*.exe
    git commit -m "chore: 备份 v{版本号}"

4.2 增量备份
----

日常开发仅提交源码，不重复打包::

    git add -A
    git commit -m "feat: 描述本次变更"
    git push

----

5. 恢复验证
====

在任意 Windows 10/11 x64 机器上执行以下步骤，验证备份可用性:

**5.1 从源码恢复::

    # 1. 解压源码包
    Expand-Archive -Path .\bak\source\MDEditor-src-*.zip -DestinationPath C:\Project\MDE\

    # 2. 安装 .NET 10 SDK
    winget install --id Microsoft.DotNet.SDK.10 -e

    # 3. 还原并构建
    cd C:\Project\MDE
    dotnet restore
    dotnet build -c Release

    # 4. 验证框架依赖版启动
    dotnet run

**5.2 从发布产物直接运行::

    # 无需安装任何依赖，直接双击或命令行启动
    .\publish\MD文本编辑器v*.exe

**5.3 验收清单:**

- [ ] 程序窗口正常显示（最大化启动）
- [ ] 菜单栏四色指示器可见（☰◎◼#）
- [ ] 可新建标签页（Ctrl+N）
- [ ] 可输入 Markdown 文本并预览
- [ ] 可切换视图模式（Ctrl+1~7）
- [ ] 可切换主题（Ctrl+Shift+D）
- [ ] 关闭后重新打开，会话恢复

----

6. 版本对应关系
====

| 版本号 | 日期 | 源码包 | 发布产物 |
|--------|------|--------|----------|
| v2.57.202605242121 | 2026-05-24 21:21 | ``source/MDEditor-src-20260524.zip`` | ``publish/MD文本编辑器v2.57.202605242121.exe`` |
| v1.28.202605241532 | 2026-05-24 15:32 | — | — |

> **版本号格式:** ``v{主版本}.{次版本}.{日期时间}``
> 主版本 ``2`` = 大版本更新，``1`` = 正式版，``0`` = 测试版。
> 详见 `VERSION.md <VERSION.md>`_。

----

7. 注意事项
====

1. **排除编译产物**: ``bin/`` ``obj/`` ``.vs/`` 不得打包进源码包
2. **产物体积**: 自包含 ``.exe`` 约 174 MB (内嵌 .NET 10 运行时 + WPF 框架)
3. **源码包不含 bak/ 自身**: 避免 ZIP 递归嵌套
4. **用户配置不备份**: ``%LOCALAPPDATA%\MDEditor\settings.json`` 与
   ``session.json`` 存储在用户目录，跨机器恢复时需重新配置主题、字体等偏好
5. **发布前确认代码已提交**: 源码包内容应与 Git 仓库状态一致
6. **文件命名不含连字符**: ``MD文本编辑器v2.57.202605242121.exe`` 而非
   ``MD文本编辑器-v2.57-202605242121.exe``

----

8. 相关文档
====

- `README.md <README.md>`_ — 项目说明与技术架构
- `VERSION.md <VERSION.md>`_ — 版本号规则与历史
- `CHANGELOG.md <CHANGELOG.md>`_ — 详细更新日志
- `文件操作逻辑.md <文件操作逻辑.md>`_ — 文件操作业务分析
- `视图显示逻辑.md <视图显示逻辑.md>`_ — 视图布局业务分析
- `四色条切换逻辑.md <四色条切换逻辑.md>`_ — 面板切换业务分析
- `业务逻辑分类总结.md <业务逻辑分类总结.md>`_ — 全项目业务逻辑索引

