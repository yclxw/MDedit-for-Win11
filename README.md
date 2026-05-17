# MD文本编辑器 for Win11

轻量级 Markdown 编辑器，基于 .NET 8 + WPF，专为 Windows 10/11 设计。
开发工具：Visual Studio Code + DeepSeek v4.0 pro
---

## 系统要求


| 场景 | 要求 |
|------|------|
| 运行 | Windows 10 / 11 + [.NET 8 Runtime](https://dotnet.microsoft.com/download/dotnet/8) |
| 构建 | Windows + [.NET 8 SDK](https://dotnet.microsoft.com/download/dotnet/8)（或 `winget install --id Microsoft.DotNet.SDK.8 -e`） |
| 预览引擎 | WPF 内置 `WebBrowser` 控件（IE Trident），无需额外依赖 |



## 功能清单

### 文件

| 功能 | 快捷键 | 说明 |
|------|--------|------|
| 新建 | `Ctrl+N` | |
| 打开 | `Ctrl+O` | 支持 .md / .txt |
| 打开文件夹 | — | 加载工作区，路径自动记忆，下次启动自动恢复 |
| 保存 | `Ctrl+S` | |
| 另存为 .md | `Ctrl+Shift+S` | |
| 另存为 .txt | — | 渲染为纯文本后保存，默认使用当前文档名 |
| 另存为 HTML | — | 导出完整 HTML 文件，默认使用当前文档名 |
| 打印 | `Ctrl+P` | 调用系统打印对话框，打印渲染后的预览内容 |
| 关闭 | `Ctrl+W` | 关闭当前页签 |
| 最近打开的文件 | — | 保留最近 20 条 |
| 退出 | — | |

### 编辑

| 功能 | 快捷键 | 说明 |
|------|--------|------|
| 撤销 / 重做 | `Ctrl+Z` / `Ctrl+Y` | TextBox 原生支持 |
| 剪切 / 复制 / 粘贴 | `Ctrl+X` / `Ctrl+C` / `Ctrl+V` | TextBox 原生支持 |
| 复制为纯文本 | `Ctrl+Shift+C` | 剥离 Markdown 语法 |
| 复制为 Markdown | — | 保留原始 Markdown 源码 |
| 粘贴为纯文本 | `Ctrl+Shift+V` | 剥离格式粘贴 |
| 全选 | `Ctrl+A` | |
| 查找/替换 | `Ctrl+F` | 支持区分大小写和正则表达式 |
| 跳转到行 | `Ctrl+G` | 弹出输入框跳转 |

### 视图

| 功能 | 快捷键 | 说明 |
|------|--------|------|
| 仅源码 | `Ctrl+1` | |
| 仅预览 | `Ctrl+2` | |
| 双栏（左右） | `Ctrl+3` | 纵向分割，可拖拽 |
| 双栏（上下） | `Ctrl+4` | 横向分割，可拖拽 |
| 全屏 | `F11` | 窗口最大化切换（默认启动即最大化） |
| 侧边栏 | `Ctrl+Shift+B` | 显示/隐藏 |
| 大纲 | `Ctrl+Shift+O` | 显示/隐藏，点击标题跳转到对应行 |
| 文件树 | `Ctrl+Shift+E` | 显示/隐藏 |
| 行号 | `Ctrl+Shift+G` | 显示/隐藏 |
| 放大 / 缩小 / 重置 | `Ctrl+=` / `Ctrl+-` / `Ctrl+0` | 字体缩放 |
| 深色/浅色主题 | `Ctrl+Shift+D` | 全局主题 + 预览 CSS 同步 |

### 格式

| 功能 | 快捷键 | 插入内容 |
|------|--------|----------|
| 标题 1–6 | `Ctrl+Shift+1~6` | `# ` ~ `###### ` |
| 粗体 | `Ctrl+B` | `**text**` |
| 斜体 | `Ctrl+I` | `*text*` |
| 删除线 | `Ctrl+Shift+X` | `~~text~~` |
| 行内代码 | `` Ctrl+` `` | `` ` `` `` ` `` |
| 代码块 | `` Ctrl+Shift+` `` | ` ``` ` |
| 引用块 | `Ctrl+Shift+Q` | `> ` |
| 无序列表 | `Ctrl+Shift+U` | `- ` |
| 有序列表 | `Ctrl+Shift+N` | `1. ` |
| 任务列表 | `Ctrl+Shift+T` | `- [ ] ` |
| 链接 | `Ctrl+Shift+K` | `[text](url)` |
| 图片 | `Ctrl+Shift+I` | `![alt](url)` |
| 表格 | — | 两列表格模板 |
| 水平分割线 | — | `---` |

> 有选中文本时包裹选中内容，未选中时插入模板。格式操作采用 150ms 防抖渲染，连续操作无卡顿。

### 工具

| 功能 | 说明 |
|------|------|
| 字数统计 | 弹窗显示字数 |
| 生成目录 | 扫描全文标题，生成带锚点链接的目录列表插入光标位置 |

### 页签

- 多标签页编辑，独立文档
- 脏标记：未保存时标题显示 `*`
- 右键页签菜单：关闭当前 / 关闭其他 / 关闭全部
- 每个页签含关闭按钮

---

## 项目结构

```
C:\Vs\MDE\
├── App.xaml / App.xaml.cs         # 应用入口
├── MainWindow.xaml                # 主窗口布局
├── MainWindow.xaml.cs             # 核心逻辑、事件处理、快捷键注册
├── FindReplaceWindow.xaml/.cs     # 查找/替换对话框
├── MDEditor.csproj                # 项目文件
├── build.ps1                      # 构建脚本
├── Att\                           # 图标等资源
├── Editor\
│   └── MarkdownEditor.xaml/.cs    # 编辑器控件（TextBox + 行号栏）
├── Models\
│   └── Document.cs                # 文档数据模型
├── Services\
│   ├── ClipboardService.cs        # 剪贴板服务
│   ├── FileService.cs             # 文件读写
│   ├── MarkdownConverter.cs       # Markdown → HTML（Markdig）
│   └── SettingsService.cs         # 设置持久化（JSON）
└── ExE\                           # 构建输出目录
```

---

## 架构设计

- **UI 模式**：代码后置（Code-Behind），无 MVVM 框架
- **服务层**：`Services/` 下全为静态方法类
- **Markdown 渲染**：`Markdig` 管道（AdvancedExtensions + PipeTables + TaskLists + AutoLinks + Mathematics），生成内嵌 CSS 的完整 HTML
- **预览引擎**：WPF `WebBrowser` 控件（IE Trident），通过 `NavigateToString()` 实时刷新，150ms 防抖避免频繁重载
- **编辑器**：自研 `UserControl` 封装 WPF `TextBox`，手动实现行号栏、查找/替换、大纲跳转
- **快捷键**：`RoutedUICommand` + `CommandBinding` + `KeyBinding` 注册到 Window 级别，全部 30+ 组组合键实际可用
- **设置**：`%LOCALAPPDATA%\MDEditor\settings.json`（主题、最近文件、上次打开文件夹）

---

## 依赖

| 包 | 版本 | 用途 |
|----|------|------|
| [Markdig](https://github.com/xoofx/markdig) | 0.34.0 | Markdown → HTML |
| [HtmlAgilityPack](https://html-agility-pack.net/) | 1.11.60 | HTML 解析 |

框架：`net8.0-windows` + WPF + Windows Forms（仅用于 `FolderBrowserDialog`）

---

## 注意事项

1. **预览引擎**：`WebBrowser` 基于 IE 引擎，部分现代 CSS 特性（`prefers-color-scheme`、CSS 变量、`display: grid`）不支持，KaTeX 数学公式和 Mermaid 图表不会渲染，仅显示原始代码
2. **文件锁定**：构建前请确保 `MDEditor.exe` 未运行（`build.ps1` 已含自动 kill）
3. **快捷键**：`Ctrl+1~4` 为视图模式，`Ctrl+Shift+1~6` 为标题插入，不同修饰键不冲突；`Ctrl+B` 为粗体
4. **编码**：所有文件以 UTF-8 读写
5. **启动**：默认窗口最大化，不自动创建新文件；打开文件夹的路径自动记忆
6. **设置重置**：删除 `%LOCALAPPDATA%\MDEditor\` 目录即可
7. **独立部署**：`dotnet publish -r win-x64 --self-contained -p:PublishSingleFile=true -p:AssemblyName="MD文本编辑器" -o ./bak` 可打包为单文件 exe
