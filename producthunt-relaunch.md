# Product Hunt Relaunch Kit — KinetAios v1.5

> **核心定位调整：Direct Engine（Kaios）是主角。** Claude Code / Codex 只是可选的外接引擎，不是卖点。
>
> 主打叙事：一个自研的 ReAct Agent 引擎 + 12 个内置工具 + 本地记忆，跑在你自己的桌面上。不依赖任何特定 CLI，不需要装 Claude Code / Codex。你的 API key → 你的模型 → 你的 agent。

---

## 1. Tagline（60 chars max）

**Your own AI agent engine — no CLI, no cloud**

备选：
- A self-hosted ReAct agent with 12 tools and real memory
- Run a real AI agent on your desktop — not a chatbot skin

---

## 2. Subtitle / Description（260 chars max）

KinetAios ships its own ReAct agent engine with 12 built-in tools (shell, file I/O, git, web search, memory recall, sub-agent dispatch), cross-session long-term memory, file snapshots with one-click rollback, plugin system, and Ollama support. No CLI dependencies. No account.

---

## 3. Topics

- Artificial Intelligence
- Developer Tools
- Open Source

---

## 4. The Pitch — Why "Direct Engine"?

> 这一整段可以作为 Maker Comment 的核心内容。

Most "AI agent" apps today fall into one of two camps:

1. **Thin chat wrappers** — they forward your message to an API and stream tokens back. No real tools, no file system access, no memory beyond the context window.
2. **CLI launchers** — they spawn `claude` or `codex` as a subprocess and forward the output. If you don't have those CLIs installed (or don't want to pay for their subscriptions), you get nothing.

**KinetAios is neither.**

At its core is **Direct Engine** (we call it "Kaios") — a from-scratch ReAct agent loop built in TypeScript that:

- **Calls any OpenAI-compatible or Anthropic-compatible API** — GLM, DeepSeek, Qwen, Ollama, OpenAI, Mistral, any provider. One API key, no vendor lock-in.
- **Has 12 built-in tools that run on your machine** — not JSON stubs, real Node.js implementations:
  - `shell` — execute any command (with user confirmation + sandbox modes)
  - `read_file` / `write_file` / `edit_file` — full file system I/O with auto-snapshot before every write
  - `grep` / `glob` — search files like a developer
  - `git_diff` — inspect uncommitted changes, per-file or per-ref
  - `web_search` — multi-engine fallback (Bing + DuckDuckGo)
  - `web_fetch` — fetch and denoise any URL (Jina Reader + native fetch)
  - `recall_memory` — search long-term memory (FTS5 full-text + embedding)
  - `dispatch_agent` — spawn a sub-agent for parallel exploration
- **Manages its own context window** — reactive trim (auto-shrink on context overflow) + history compaction (LLM-summarize old turns instead of dropping them) + orphan tool-call cleanup. Long conversations don't silently break.
- **Auto-extracts long-term memory** — after every turn, a background pass pulls durable facts into SQLite. Next session — even with a different model — gets them injected automatically.
- **Snapshots every file write** — each `write_file` / `edit_file` snapshots the original content to `.kinet-snapshots/`. One-click rollback. The agent can't destroy your code.

**Claude Code and Codex?** They're available as *optional* engines — you can switch to them per-conversation if you happen to have them installed. But they're not required. They're not the point. The point is: **KinetAios gives you a real agent that works with any model, out of the box, with zero dependencies.**

---

## 5. Maker Comment（发布后立即发）

Hey Product Hunt! 👋

We first launched KinetAios a few months ago. The feedback was clear: people loved the idea but wanted to understand *what actually makes it an agent, not just a chat UI*.

So let me be direct about that.

**KinetAios has its own agent engine.** Not a wrapper around Claude Code. Not a launcher for Codex. A from-scratch ReAct loop — model calls a tool → tool executes on your machine → result feeds back → model decides next step — running entirely inside the app.

**12 built-in tools, all real Node.js implementations:**

| Tool | What it does |
|------|-------------|
| `shell` | Execute any shell command (with user confirmation) |
| `read_file` | Read any file, UTF-8 |
| `write_file` | Write file + auto-snapshot before write |
| `edit_file` | Find-and-replace in a file + auto-snapshot |
| `grep` | Recursive regex search (like ripgrep) |
| `glob` | File pattern matching |
| `git_diff` | View uncommitted changes (working tree / staged / per-ref) |
| `web_search` | Multi-engine search (Bing + DuckDuckGo fallback) |
| `web_fetch` | Fetch URL → clean Markdown (Jina Reader + native) |
| `recall_memory` | Search extracted long-term memory |
| `dispatch_agent` | Spawn a read-only sub-agent for parallel tasks |

**Context management that actually works:**
- Reactive trim — if the model's context window overflows mid-turn, auto-shrink and retry (don't just crash)
- History compaction — instead of dropping old messages, LLM-summarize them into a compact paragraph
- Orphan cleanup — if a tool call gets trimmed, its orphan result gets cleaned up too

**Long-term memory:**
- After every turn, a background extraction pass pulls durable facts into SQLite + FTS5
- Next session automatically injects relevant memories — regardless of which model you use
- Full transparency: a force-directed **Memory Graph** shows every extracted fact, its source conversation, and conflicts (marked red)

**Works with any provider:**
- GLM (Zhipu), DeepSeek, Qwen, Mistral — OpenAI-compatible API
- Claude (Anthropic protocol) — with cache_control support for system prompts
- **Ollama** — native `/api/chat` endpoint, auto-detect local models, 32K context

**Safety:**
- Every `write_file` / `edit_file` auto-snapshots the original → one-click rollback
- Shell commands require user confirmation (configurable: always / never / ask)
- Sandbox modes: `readOnly` / `workspaceWrite` / `fullAccess`
- SSRF protection on `web_fetch` (blocks internal IPs, DNS rebinding)
- Error messages sanitized before returning to the model (no path/username leaks)

**Plugin system extends the toolset:**
- Drop a folder → agent gets new tools, slash commands, system prompts
- Ships with an office suite plugin (Excel, PDF, OCR, doc conversion — 15 tools)
- Per-conversation system prompt injection (keyword-matched, saves ~60% tokens)

We also support Claude Code and Codex as *optional* engines — switch per-conversation if you have them installed. But you don't need them. The Direct Engine is the product.

What would you build with a real agent engine on your desktop? 🙏

---

## 6. Gallery Assets — 需要准备的图/GIF

| # | 类型 | 内容 | 说明 |
|---|------|------|------|
| 1 | **Hero (1270×760)** | Direct Engine ReAct loop 示意图 + 工具列表 | 核心卖点：这是一个 *真正的 agent* |
| 2 | **GIF** | Agent 调用工具链：web_search → read_file → write_file → 完成任务 | 展示 ReAct loop 实际运行 |
| 3 | **Screenshot** | Memory Graph 力导向图 | "AI 记住了什么，全透明" |
| 4 | **GIF** | 文件快照回滚：agent 改文件 → 快照面板 → 一键 restore | 安全感：agent 破坏不了你的代码 |
| 5 | **Screenshot** | Context Inspector — 看到 trim/compact 后的完整消息 + token 进度条 | 上下文管理透明化 |
| 6 | **Screenshot** | 设置页：API Key + Provider 切换 + Ollama 配置 + Model Profiles | "any provider, any model" |
| 7 | **Screenshot** | Plugin 系统拖入安装 → 工具列表扩展 | 可扩展性 |

---

## 7. First Comment（发布后自己发，引导讨论）

The thing that surprised me most while building this: **context management is harder than the agent loop itself.**

Anyone can write a while-loop that calls a model and checks for tool_calls. The hard part is:

1. **What happens when you hit the context limit mid-turn?** Most agents just crash or silently truncate. We do reactive trim — halve the budget, retry the turn — then compact old history into an LLM-generated summary.

2. **What happens to orphaned tool results?** If you trim the assistant message that made a tool call, the tool's response becomes an orphan and corrupts the next API call. We do a two-pass cleanup.

3. **How do you persist memory across sessions?** Not vector DBs. Just structured extraction + FTS5 keyword search. Simple, fast, transparent.

These are unglamorous engineering problems. But they're what separate a "demo agent" from one you can actually use for hours of real work.

Curious — what context/memory problems have you hit with AI agents? Has anyone found a solution they're happy with?

---

## 8. Twitter / X Launch Thread

**Tweet 1:**
Most "AI agent" apps are either thin chat wrappers or CLI launchers.

KinetAios is neither. It has its own ReAct agent engine — 12 real tools, cross-session memory, file snapshots, works with any LLM provider.

Now on @ProductHunt 🚀

🧵👇

[#1]

**Tweet 2:**
🔧 12 built-in tools — not JSON stubs, real Node.js:

shell · read_file · write_file · edit_file · grep · glob · git_diff · web_search · web_fetch · recall_memory · dispatch_agent

The agent can search your codebase, edit files, run commands, fetch docs, and remember what it learned.

[#2]

**Tweet 3:**
🧠 Long-term memory that actually works:

After every turn → auto-extract durable facts → SQLite + FTS5 → inject on next session

Works across models. Fully transparent — there's a force-directed graph showing everything the AI knows about you.

[#3]

**Tweet 4:**
💾 Every file write is auto-snapshotted.

The agent edits your code → original content saved to .kinet-snapshots/ → one-click rollback.

Your AI can't destroy your work. Ever.

[#4]

**Tweet 5:**
⚙️ Context management that doesn't break:

✅ Reactive trim — overflow mid-turn? Auto-shrink + retry
✅ History compaction — LLM-summarize old turns (don't just drop them)
✅ Orphan cleanup — no dangling tool results

Long conversations just work.

[#5]

**Tweet 6:**
🔌 Works with ANY provider:

✅ GLM / DeepSeek / Qwen / Mistral (OpenAI API)
✅ Claude (Anthropic protocol, cache_control)
✅ Ollama — native /api/chat, auto-detect models, 32K context, fully offline

No vendor lock-in. Your API key = the only credential.

[#6]

**Tweet 7:**
🧩 Plugin system extends the tools:

Drop a folder → agent learns new skills. Ships with:
📊 Office suite (Excel, PDF, OCR — 15 tools)
🎓 C++ learning module
📐 Math practice

Custom tools, slash commands, system prompts, panels.

[#7]

**Tweet 8:**
Claude Code and Codex are available as optional engines — switch per-conversation if you have them.

But you don't need them. The built-in Direct Engine IS the product.

👉 github.com/phinn/KinetAios
🚀 Live on Product Hunt — come say hi!

[#8]

---

## 9. Reddit Title

**KinetAios v1.5: I built an open-source desktop AI agent with its own ReAct engine — 12 built-in tools, cross-session memory, file snapshots, works with any LLM provider (Ollama included). No CLI dependencies.**

---

## 10. Hacker News Title

**Show HN: KinetAios – Desktop AI agent with a from-scratch ReAct engine, 12 tools, and long-term memory (GPL v3)**

---

## 11. LinkedIn Post

Excited to relaunch KinetAios on Product Hunt! 🚀

Most AI apps today are either thin chat wrappers or launchers for someone else's CLI. KinetAios is different — it has its **own agent engine** built from scratch in TypeScript.

The engine runs a ReAct loop: the model calls a tool → the tool executes on your machine → the result feeds back → the model decides the next step. Repeat until the task is done.

**12 built-in tools:** shell, file read/write/edit, grep, glob, git diff, web search, web fetch, memory recall, and sub-agent dispatch. All real implementations — not JSON stubs.

**Long-term memory:** auto-extracts durable facts after every conversation → SQLite + FTS5 → injects them on the next session, regardless of which model you use.

**Works with any provider:** GLM, DeepSeek, Qwen, Claude, Ollama — anything OpenAI-compatible or Anthropic-compatible.

**Safety:** every file write is auto-snapshotted with one-click rollback. Shell commands require user confirmation. Sandbox modes for read-only or workspace-limited access.

Open source (GPL v3). Windows + macOS. No account, no telemetry.

If you've wanted a real AI agent on your desktop — not just a chat UI — I'd love your feedback.

🔗 github.com/phinn/KinetAios

---

## 12. Key Differentiators — Direct Engine Deep Dive

### 为什么 Direct Engine 不是"又一个 chatbot"

| 维度 | Thin Chat Wrapper | CLI Launcher (e.g. Claude Code wrapper) | **KinetAios Direct Engine** |
|------|-------------------|-----------------------------------------|---------------------------|
| 工具调用 | ❌ 无 | ✅ 但依赖外部 CLI | ✅ 12 个内置 Node.js 工具 |
| 模型绑定 | 单一 provider | 绑定 CLI 的 provider | **任意 OpenAI/Anthropic/Ollama** |
| 上下文管理 | 无（到限就截断） | CLI 内部处理 | **Reactive trim + LLM compaction + orphan cleanup** |
| 文件安全 | ❌ | ❌ | **每次写入自动快照 + 一键回滚** |
| 长期记忆 | ❌ | ❌ | **自动提取 → SQLite + FTS5 → 跨会话注入** |
| 沙箱 | ❌ | ❌ | **readOnly / workspaceWrite / fullAccess** |
| SSRF 防护 | ❌ | ❌ | **内网/元数据/DNS rebinding 拦截** |
| 可扩展 | ❌ | 插件依赖 CLI | **插件系统 V2（工具/命令/面板/prompt）** |
| 安装要求 | 只需 API key | 需要安装 CLI + 可能需订阅 | **只需 API key** |

### Direct Engine 技术架构（一句话版）

> `AgentLoop.run()` → `provider.streamComplete()` → 解析 `tool_calls` → `allTools().find(t => t.name === call.name).run(args)` → 结果拼回 messages → 下一轮 → 直到无 tool_call → done。

### 上下文管理三重保障

1. **Reactive Trim** — 当 API 返回 context overflow 错误时，自动将 history 砍半并重试本轮。不会崩溃，不会丢用户消息。
2. **History Compaction** — 当 history 超过 token budget 时，不是简单丢弃头部消息，而是调一次 LLM 把旧对话压成一段摘要。早期上下文不丢失。
3. **Orphan Cleanup** — 如果 trim 删掉了某个 assistant 的 tool_call 消息，对应的 tool 结果消息也会被清理。防止 API 报 "tool message without corresponding tool_call" 错误。

### 12 个工具一览

| 工具 | 功能 | 安全措施 |
|------|------|----------|
| `shell` | 执行任意命令 | 用户确认 + approval 策略 |
| `read_file` | 读取 UTF-8 文件 | readOnly 沙箱限制在 cwd 内 |
| `write_file` | 写入文件 | **写入前自动快照** + workspaceWrite 限制 |
| `edit_file` | 精确查找替换 | **写入前自动快照** + 严格匹配校验 |
| `grep` | 递归正则搜索 | 自动排除 node_modules/.git 等 |
| `glob` | 文件模式匹配 | 只读 |
| `git_diff` | 查看 git 改动 | 只读 |
| `web_search` | 多引擎搜索 | Bing 中国版 + DDG 回退 |
| `web_fetch` | 抓取 URL 正文 | **SSRF 防护** + Jina Reader 优先 + HTML 去噪 |
| `recall_memory` | 搜索长期记忆 | FTS5 + embedding 语义召回 |
| `dispatch_agent` | 派发只读子 agent | 子 agent 无 shell/write 权限，防递归 |

---

## 13. Pre-Launch Checklist

### The Day Before
- [ ] Product Hunt 产品页更新：tagline、description、gallery 全部聚焦 Direct Engine
- [ ] 准备 7 张 gallery 图/GIF（见 Section 6）—— **至少 2 张展示 ReAct loop 实际跑工具链**
- [ ] GitHub README 首屏加一段 "What is Direct Engine?" 的说明
- [ ] GitHub Release v1.5.0 release notes 以 Direct Engine 为主轴
- [ ] 确认安装包可正常安装（Windows + macOS）
- [ ] 通知 supporters，准备 launch 日的 upvote + comment

### Launch Day（PST 00:01）
- [ ] 正式 launch
- [ ] 立即发 Maker Comment（Section 5）
- [ ] 立即发 First Comment（Section 7）
- [ ] 发 Twitter thread（Section 8）
- [ ] 发 LinkedIn（Section 11）
- [ ] 提交 Hacker News（Section 10）
- [ ] 发 Reddit（Section 9）
- [ ] 通知所有 supporters
- [ ] 回复每一条 PH 评论（前 12 小时最关键）

### Launch Day +1
- [ ] 感谢所有支持者
- [ ] GitHub 发布 launch 后总结
- [ ] 收集反馈，整理 issue

---

## 14. 竞品对比表（Gallery 第 1 张图用）

|  | **KinetAios (Direct)** | Claude Desktop | Cursor | ChatGPT Desktop | Cherry Studio |
|---|---|---|---|---|---|
| 自研 ReAct 引擎（非 wrapper） | ✅ | — | — | — | — |
| 12 个内置本地工具 | ✅ | — | — | — | — |
| 文件写入自动快照 + 回滚 | ✅ | — | — | — | — |
| 跨会话长期记忆 (SQLite + FTS5) | ✅ | — | — | — | — |
| 任意 LLM provider（无锁定） | ✅ | — | — | — | ✅ |
| Ollama 原生支持 | ✅ | — | — | — | ✅ |
| 沙箱模式 (readOnly/workspaceWrite) | ✅ | — | — | — | — |
| SSRF 防护 | ✅ | — | — | — | — |
| 插件系统（自定义工具/命令/面板） | ✅ | — | — | — | — |
| 子 agent 派发 | ✅ | — | — | — | — |
| 上下文压缩（LLM 摘要） | ✅ | — | — | — | — |
| 无账号，无遥测 | ✅ | — | — | — | ✅ |
| 开源 (GPL v3) | ✅ | — | — | — | ✅ |
