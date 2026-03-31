# Claude Code 源码学习仓库

> 本仓库保存了 Claude Code（AI 编程协作工具）的核心源代码，供学习与研究使用。

---

## 📁 项目总览

本项目是一个基于 **TypeScript / React (Ink)** 构建的 AI 编程协作命令行工具，采用模块化设计，主要运行在终端（Terminal）环境中。其核心能力包括：与 AI 模型交互、执行工具调用、管理会话历史、桥接远程环境等。

---

## 🗂️ 目录结构说明

### 核心入口文件

| 文件 | 说明 |
|------|------|
| `main.tsx` | 应用主入口，约 785KB，负责整体初始化和渲染 |
| `query.ts` | AI 查询引擎核心逻辑，处理与模型的通信 |
| `QueryEngine.ts` | 查询引擎封装，管理上下文与请求调度 |
| `Tool.ts` | 工具基类定义，所有工具的抽象父类 |
| `Task.ts` | 任务模型定义 |
| `commands.ts` | 命令注册与调度中心 |
| `tools.ts` | 工具注册与管理 |
| `history.ts` | 会话历史管理 |
| `setup.ts` | 应用初始化配置 |
| `context.ts` | 上下文管理 |
| `cost-tracker.ts` | Token 消耗与费用追踪 |
| `interactiveHelpers.tsx` | 交互式 UI 辅助组件 |

---

### 📂 主要目录模块

#### `bridge/` — 桥接通信模块
负责 Claude Code 与远程环境（如 Web IDE、移动端）之间的通信桥接。

| 文件 | 功能 |
|------|------|
| `replBridge.ts` | REPL 桥接核心，约 98KB |
| `bridgeMain.ts` | 桥接主逻辑，约 113KB |
| `remoteBridgeCore.ts` | 远程桥接核心实现 |
| `bridgeMessaging.ts` | 消息收发管理 |
| `bridgeApi.ts` | 桥接 API 接口定义 |
| `trustedDevice.ts` | 可信设备认证管理 |
| `jwtUtils.ts` | JWT 令牌工具 |
| `sessionRunner.ts` | 会话运行管理 |
| `createSession.ts` | 会话创建逻辑 |

---

#### `tools/` — 工具执行模块
每个子目录对应一类工具，AI 可以调用这些工具完成具体操作：

| 工具目录 | 功能描述 |
|----------|----------|
| `BashTool/` | 执行 Bash 命令（约 18 个文件） |
| `PowerShellTool/` | 执行 PowerShell 命令（Windows 支持） |
| `FileReadTool/` | 读取文件内容 |
| `FileEditTool/` | 编辑文件内容 |
| `FileWriteTool/` | 写入新文件 |
| `GlobTool/` | 文件路径通配符匹配 |
| `GrepTool/` | 文件内容正则搜索 |
| `WebFetchTool/` | 网页内容抓取 |
| `WebSearchTool/` | 网络搜索 |
| `AgentTool/` | 启动子 Agent 执行任务 |
| `MCPTool/` | MCP（Model Context Protocol）协议工具 |
| `LSPTool/` | 语言服务协议（代码补全/跳转）工具 |
| `TaskCreateTool/` | 创建异步任务 |
| `TaskListTool/` | 列出任务列表 |
| `TaskGetTool/` | 获取任务状态 |
| `TaskUpdateTool/` | 更新任务状态 |
| `TaskStopTool/` | 停止任务 |
| `TodoWriteTool/` | 写入 Todo 任务列表 |
| `SkillTool/` | 执行技能脚本 |
| `TeamCreateTool/` | 创建协作团队（Swarm 模式） |
| `TeamDeleteTool/` | 删除协作团队 |
| `SendMessageTool/` | 向 Teammate 发送消息 |
| `ScheduleCronTool/` | 定时任务调度 |
| `NotebookEditTool/` | Jupyter Notebook 编辑 |
| `REPLTool/` | REPL 交互执行 |
| `AskUserQuestionTool/` | 向用户提问确认 |
| `EnterPlanModeTool/` | 进入计划模式 |
| `ExitPlanModeTool/` | 退出计划模式 |
| `EnterWorktreeTool/` | 进入 Git Worktree |
| `RemoteTriggerTool/` | 远程触发操作 |
| `ConfigTool/` | 读写配置项 |

---

#### `commands/` — 命令模块
包含所有 `/` 斜杠命令（slash commands）的实现，超过 80 个命令目录：

| 类别 | 包含命令 |
|------|----------|
| **会话管理** | `session/`, `resume/`, `clear/`, `compact/`, `export/`, `rewind/` |
| **代码操作** | `commit/`, `diff/`, `review/`, `branch/`, `rename/` |
| **认证与账户** | `login/`, `logout/`, `oauth-refresh/` |
| **配置管理** | `config/`, `model/`, `theme/`, `color/`, `effort/` |
| **插件与技能** | `plugin/`, `skills/`, `reload-plugins/` |
| **MCP 集成** | `mcp/` |
| **任务与规划** | `plan/`, `tasks/`, `ultraplan.tsx` |
| **调试诊断** | `doctor/`, `debug-tool-call/`, `heapdump/`, `stats/` |
| **安全审查** | `security-review.ts`, `permissions/`, `privacy-settings/` |
| **GitHub 集成** | `install-github-app/`, `install-slack-app/` |
| **远程环境** | `remote-env/`, `remote-setup/`, `teleport/` |
| **多媒体** | `voice/`, `stickers/` |
| **帮助信息** | `help/`, `version.ts`, `release-notes/` |
| **其他** | `memory/`, `context/`, `ide/`, `vim/`, `feedback/` |

---

#### `services/` — 后台服务模块

| 目录/文件 | 功能 |
|-----------|------|
| `api/` | 与 Claude API 的通信服务（20 个文件） |
| `mcp/` | MCP 协议服务端实现（23 个文件） |
| `oauth/` | OAuth 认证流程 |
| `compact/` | 对话压缩与摘要服务 |
| `lsp/` | 语言服务器协议集成 |
| `analytics/` | 使用数据分析 |
| `SessionMemory/` | 会话记忆提取 |
| `extractMemories/` | 从对话中提取记忆 |
| `voice.ts` | 语音交互服务 |
| `voiceStreamSTT.ts` | 语音流式识别（STT） |
| `tokenEstimation.ts` | Token 数量估算 |
| `claudeAiLimits.ts` | Claude AI 速率限制管理 |
| `plugins/` | 插件服务管理 |
| `settingsSync/` | 配置同步服务 |
| `teamMemorySync/` | 团队记忆同步 |
| `remoteManagedSettings/` | 远程托管设置 |

---

#### `utils/` — 工具函数库
项目中最大的模块，包含 **300+ 个工具函数文件**，涵盖：

| 类别 | 典型文件 |
|------|----------|
| **Git 操作** | `git.ts`, `gitDiff.ts`, `detectRepository.ts` |
| **文件系统** | `file.ts`, `fsOperations.ts`, `fileHistory.ts` |
| **会话管理** | `sessionStorage.ts` (176KB), `sessionRestore.ts`, `sessionState.ts` |
| **认证安全** | `auth.ts` (64KB), `permissions/`, `secureStorage/` |
| **配置系统** | `config.ts` (62KB), `settings/` |
| **模型管理** | `model/`, `modelCost.ts`, `betas.ts` |
| **Shell 执行** | `Shell.ts`, `ShellCommand.ts`, `shell/` |
| **Swarm 多智能体** | `swarm/`（多 Agent 协作） |
| **终端渲染** | `Cursor.ts`, `terminal.ts`, `theme.ts` |
| **图像处理** | `imageResizer.ts`, `imagePaste.ts`, `ansiToPng.ts` |
| **遥测与日志** | `telemetry/`, `log.ts`, `debug.ts` |
| **插件系统** | `plugins/` (44 个文件) |
| **Cron 定时任务** | `cron.ts`, `cronScheduler.ts`, `cronTasks.ts` |
| **MCP 工具** | `mcp/`, `mcpWebSocketTransport.ts` |
| **工作区管理** | `worktree.ts`, `ide.ts` |
| **Teleport 功能** | `teleport.tsx` (172KB), `teleport/` |

---

#### `hooks/` — React Hooks 模块
包含 **80+ 个自定义 Hooks**，供 UI 组件使用：
- 快捷键绑定、主题切换、权限管理、状态订阅等

#### `components/` — UI 组件库
包含 **140+ 个 React 组件**（基于 Ink 终端渲染框架）

#### `ink/` — Ink 框架适配
包含对 Ink（终端 React 渲染库）的封装和扩展（50 个文件）

#### `buddy/` — AI 伴侣模块
AI 助手的动画精灵（Sprite）和通知功能

#### `keybindings/` — 快捷键系统
自定义快捷键的解析、绑定、验证与用户配置

#### `memdir/` — 记忆目录管理
管理 AI 的长期记忆文件（CLAUDE.md 等）

#### `migrations/` — 数据迁移
处理版本升级时的配置项迁移（11 个迁移脚本）

#### `skills/` — 技能系统
内置技能库和技能加载器

#### `state/` — 全局状态管理
应用的全局状态定义与管理

#### `tasks/` — 任务系统
异步任务的创建、调度、执行与监控

#### `vim/` — Vim 模式支持
完整的 Vim 键位模式实现（motions、operators、textObjects 等）

#### `voice/` — 语音功能
语音模式开关控制

#### `context/` — 上下文管理
对话上下文的构建、分析与管理

#### `native-ts/` — 原生 TypeScript 绑定
- `color-diff/`：颜色差异计算
- `file-index/`：文件索引
- `yoga-layout/`：布局引擎绑定

---

## 🔧 技术栈

| 技术 | 用途 |
|------|------|
| TypeScript | 主要开发语言 |
| React + Ink | 终端 UI 渲染框架 |
| Node.js | 运行时环境 |
| MCP（Model Context Protocol） | AI 工具协议标准 |
| LSP（Language Server Protocol） | 代码智能服务 |
| WebSocket | 远程桥接通信 |
| JWT | 身份认证 |
| Zod | 运行时类型校验 |
| ripgrep | 高性能文件搜索 |

---

## 🧩 核心功能概览

```
AI编程协作者
├── 🤖 AI 对话与查询           → query.ts / QueryEngine.ts
├── 🛠️  工具调用执行             → tools/ (40+ 种工具)
├── 📁 文件操作                 → FileReadTool / FileEditTool / FileWriteTool
├── 💻 终端命令执行              → BashTool / PowerShellTool
├── 🌐 网络能力                 → WebFetchTool / WebSearchTool
├── 🔗 桥接通信                 → bridge/ (跨环境实时通信)
├── 🧠 记忆系统                 → memdir / SessionMemory / extractMemories
├── 📋 任务编排                 → TaskCreateTool / ScheduleCronTool
├── 👥 多智能体协作（Swarm）     → utils/swarm / TeamCreateTool
├── 🔌 MCP 协议集成             → services/mcp / MCPTool
├── 📝 代码语言服务              → LSPTool / services/lsp
├── 🎙️  语音交互                 → services/voice / voice/
├── 🔑 权限与安全               → utils/permissions / utils/auth
├── ⚙️  配置系统                 → utils/config / utils/settings
├── 📊 遥测与统计               → utils/telemetry / services/analytics
├── 🖊️  Vim 模式                  → vim/
└── 🎨 主题与 UI               → components / hooks / ink
```

---

## 📖 学习建议

如果你想深入学习这个项目，建议按以下顺序阅读：

1. **入门**：从 `main.tsx` 了解整体启动流程
2. **核心查询**：阅读 `query.ts` 和 `QueryEngine.ts` 了解 AI 通信机制
3. **工具系统**：阅读 `Tool.ts` 基类，再挑选几个 `tools/` 子目录深入
4. **命令系统**：阅读 `commands.ts`，再查看 `commands/` 中感兴趣的命令
5. **桥接通信**：阅读 `bridge/replBridge.ts` 和 `bridge/bridgeMain.ts`
6. **工具函数**：按需查阅 `utils/` 中的具体模块

---

## ⚠️ 声明

本仓库内容来源于 Claude Code 工具的源代码，仅供个人学习与研究使用，请勿用于商业目的。
