# KinetAios — 黑客松作品描述

---

## 项目名称

**KinetAios** — 本地优先的多引擎 AI Agent 桌面应用

---

## 一句话介绍

一个跑在你自己桌面上的 AI Agent 引擎——内置 12 个工具、跨会话长期记忆、文件快照回滚，支持任意 LLM 提供商（GLM / DeepSeek / Qwen / Claude / Ollama），无需账号、无需中继服务器、无需安装任何外部 CLI。

---

## 解决什么问题

当今 AI 工具市场存在一个核心矛盾：**大多数 "AI Agent" 不是真正的 Agent。**

它们分为两类：

1. **薄壳聊天应用**（Thin Chat Wrapper）——把你的消息转发给 API，再把 token 流回来。没有工具、没有文件系统访问、没有超出上下文窗口的记忆。本质上只是一个带 UI 的 API 客户端。

2. **CLI 启动器**（CLI Launcher）——spawn `claude` 或 `codex` 作为子进程并转发输出。如果你没装这些 CLI（或不想为它们的订阅付费），你什么都得不到。

**KinetAios 是第三条路：一个从零构建的自研 ReAct Agent 引擎。**

---

## 核心创新

### 1. 自研 ReAct Agent 引擎（Direct Engine / "Kaios"）

完整的 Reason→Act 循环，跑在 Electron 主进程里：

```
用户输入 → 模型推理（带工具定义）→ 返回 tool_calls → 执行工具 → 结果拼回上下文 → 下一轮推理 → 直到任务完成
```

- **12 个内置 Node.js 工具**（非 JSON 桩，是真实实现）：
  - `shell` — 执行任意命令（用户确认 + 沙箱）
  - `read_file` / `write_file` / `edit_file` — 完整文件 I/O，每次写入前自动快照
  - `grep` / `glob` — 递归搜索代码
  - `git_diff` — 查看 git 改动
  - `web_search` — 多引擎搜索（Bing + DuckDuckGo 回退）
  - `web_fetch` — 抓取 URL 并去噪（SSRF 防护 + Jina Reader）
  - `recall_memory` — 搜索长期记忆（FTS5 + embedding 语义召回）
  - `dispatch_agent` — 派发只读子 agent（并行探索）

### 2. 上下文管理三重保障

这是区分"演示 Agent"和"能用几小时的真 Agent"的关键：

| 机制 | 解决的问题 |
|------|-----------|
| **Reactive Trim** | 上下文溢出时自动砍半重试，不崩溃 |
| **History Compaction** | 旧对话由 LLM 压缩成摘要，不简单丢弃 |
| **Orphan Cleanup** | 被删的 tool_call 对应的孤立 tool 结果也清理，防止 API 报错 |

### 3. 跨会话长期记忆

- 每轮对话结束后，后台自动提取「关于用户的持久事实」→ 存入 SQLite + FTS5
- 下一会话自动注入相关记忆——**即使换了模型也生效**
- **记忆图谱**可视化：力导向图展示每条记忆的来源、冲突检测（红色标记）
- 记忆导入/导出 JSON

### 4. 文件安全——每次写入自动快照

Agent 的 `write_file` / `edit_file` 在改动前把原内容存到 `.kinet-snapshots/`。用户可在快照面板一键回滚。**Agent 破坏不了你的代码。**

### 5. 任意 LLM 提供商，零锁定

| 协议 | 支持的提供商 |
|------|-------------|
| OpenAI 兼容 | GLM（智谱）、DeepSeek、Qwen、Mistral、OpenAI 等 |
| Anthropic | Claude（支持 cache_control 省费） |
| Ollama 原生 | 本地 `/api/chat`，自动探测模型，32K 上下文，完全离线 |

只需 API Key，无需账号，无需中继服务器。

### 6. 插件系统 + MCP 双向集成

- **插件 v2.2**：拖入文件夹 → Agent 获得新工具、slash 命令、全屏面板。内置办公套件插件（Excel / PDF / OCR / Word — 18 个工具）
- **MCP 客户端**：自动扫描 `~/.claude.json` / Claude Desktop 配置，接入系统 MCP 服务
- **MCP 服务端**：内置 HTTP+SSE Server，远程机器可调用本机 Agent（Token 鉴权 + 超时）

### 7. 安全防护

- **沙箱模式**：`readOnly`（只读 cwd）/ `workspaceWrite`（只能写 cwd）/ `fullAccess`
- **SSRF 防护**：拦截内网 IP、云元数据端点、DNS rebinding 攻击
- **错误脱敏**：返回给模型的错误信息去掉绝对路径和用户名
- **Shell 确认桥**：危险命令弹出确认窗口，用户批准后才执行

---

## 技术架构

```
┌──────────────────────────────────────────────────┐
│                 Renderer (Vanilla TS)             │
│  Dashboard · Chat · Sidebar · Context Inspector   │
│  Memory Graph · Files Pane · Plugin Panels        │
│         (contextIsolation, no Node access)        │
└────────────────────┬─────────────────────────────┘
                     │ contextBridge (typed KinetAPI)
┌────────────────────┴─────────────────────────────┐
│              Preload (窄桥, 安全边界)               │
└────────────────────┬─────────────────────────────┘
                     │ IPC
┌────────────────────┴─────────────────────────────┐
│                Main Process (Node.js)             │
│                                                   │
│  ┌─────────────┐  ┌──────────┐  ┌─────────────┐  │
│  │ AgentLoop   │→ │ Provider │→ │  Model API  │  │
│  │ (ReAct)     │  │ (SSE)    │  │  (Stream)   │  │
│  └──────┬──────┘  └──────────┘  └─────────────┘  │
│         │                                         │
│  ┌──────┴──────────────────────────────────────┐ │
│  │  12 Tools (shell/file/grep/glob/git/web/    │ │
│  │  search/fetch/memory/dispatch_agent)         │ │
│  └─────────────────────────────────────────────┘ │
│                                                   │
│  SQLite + FTS5 · Settings (encrypted) · MCP       │
│  Plugin Loader · Snapshots · Skills Scanner       │
└───────────────────────────────────────────────────┘
```

**技术栈**：Electron + TypeScript + better-sqlite3（FTS5）+ esbuild，无前端框架（纯 vanilla TS + HTML/CSS）。

---

## 竞品对比

|  | **KinetAios** | Claude Desktop | Cursor | ChatGPT Desktop | Cherry Studio |
|---|---|---|---|---|---|
| 自研 ReAct 引擎（非 wrapper） | ✅ | — | — | — | — |
| 12 个内置本地工具 | ✅ | — | — | — | — |
| 文件写入自动快照 + 回滚 | ✅ | — | — | — | — |
| 跨会话长期记忆 | ✅ | — | — | — | — |
| 任意 LLM 提供商（零锁定） | ✅ | — | — | — | ✅ |
| Ollama 原生支持 | ✅ | — | — | — | ✅ |
| 沙箱模式 | ✅ | — | — | — | — |
| SSRF 防护 | ✅ | — | — | — | — |
| 插件系统 | ✅ | — | — | — | — |
| 子 Agent 派发 | ✅ | — | — | — | — |
| 上下文压缩 | ✅ | — | — | — | — |
| 无账号、无遥测 | ✅ | ✅ | — | — | ✅ |
| 开源（GPL v3） | ✅ | — | — | — | ✅ |

---

## 项目链接

- **GitHub**：https://github.com/phinn/KinetAios
- **官网**：https://phinn.github.io/KinetAios/
- **License**：GPL v3
- **平台**：Windows 11 + macOS

---

## 团队

独立开发者。从 macOS 原生 SwiftUI 应用移植到 Electron + TypeScript，逐行 port。

---

## 总结

KinetAios 证明了一件事：**你不需要依赖某个大公司的 CLI 就能拥有一个真正的 AI Agent。** 

一个自研的 ReAct 循环 + 12 个 Node.js 工具 + SQLite 记忆 + 文件快照安全网，就能在任何 LLM 提供商上跑出一个能实际干活的桌面 Agent。
