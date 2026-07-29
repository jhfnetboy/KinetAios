# Product Hunt Relaunch Kit — KinetAios v1.5

> 上次 launch 时的版本约 v1.2。此后新增了 40+ 个 feature commit（Town View、插件系统 V2、记忆图谱、Arena 仪表盘、Artifact 预览、Ollama 原生支持、模型配置档等）。这是全新的 launch 材料。

---

## 1. Tagline（60 chars max）

**Three AI agents, one desktop, zero cloud**

备选：
- Run Claude Code, Codex & Ollama — side by side
- Your local AI agent cockpit — three engines, one window

---

## 2. Subtitle / Description（260 chars max）

KinetAios runs Claude Code, Codex, and a built-in ReAct engine from one desktop window — with cross-engine memory, a gamified Town view, plugin system, Ollama support, and zero account. Your API key is the only auth. Open source.

---

## 3. Topics

- Artificial Intelligence
- Developer Tools
- Open Source

---

## 4. Maker Comment（发布后立即发）

Hey Product Hunt! 👋

We launched KinetAios here a few months back. The community gave incredible feedback — so we went heads-down and shipped **40+ new features**. This is the relaunch.

**What's KinetAios?** A local-first desktop app that runs three AI engines — Claude Code, Codex, and a built-in ReAct loop — from a single window. No account, no relay server. Your LLM API key is the only credential.

**What's new since last time:**

🏠 **Town View** — Your projects are pixel-art houses on an isometric map. Each conversation is a villager. Remote machines float as cloud houses. It sounds like a gimmick — it isn't. Spatial awareness of parallel agents genuinely changes how you work.

🧠 **Memory Graph** — Every extracted memory is now a node in an interactive force-directed graph. Trace any memory back to its source conversation. Detect conflicting memories (marked red). Delete what's stale. Your AI's brain, fully transparent.

⚔️ **Arena Dashboard** — One prompt → three engines racing in parallel. Side-by-side diff. AI judge picks the winner. Token-by-token cost comparison.

🔌 **Plugin System V2** — Drop a folder → your agent learns new tools, slash commands, system prompts, and full-screen panels. Ships with an office suite (15 tools: Excel, PDF, OCR, doc conversion), a C++ learning module, and a math practice module.

🎨 **Artifact Preview** — AI writes HTML/SVG → it renders live in a sandboxed iframe. Instant visual feedback, no copy-paste-to-browser.

🖥️ **Ollama Native Support** — Connect to a local Ollama instance. Auto-detects models, uses native `/api/chat` endpoint, 32K context window. Fully local, fully offline.

⚙️ **Model Profiles** — Save multiple LLM configs (provider + model + params). Switch per-conversation with a dropdown. Perfect for A/B testing models.

🎯 **Session Goals** — Set a `/goal` for each conversation. The agent stays focused on the objective.

🔍 **Visual Inspector** — Circle any element on a web preview → tell the AI "make this red" → it changes the code. No DOM inspection, no screenshot annotation.

**The philosophy hasn't changed:** No account. No telemetry. No cloud. All data in local SQLite. Open source (GPL v3). Windows + macOS.

I'd love to hear what you'd want next — what's missing from your AI workflow? 🙏

---

## 5. Gallery Assets — 需要准备的图/GIF

| # | 类型 | 内容 | 说明 |
|---|------|------|------|
| 1 | **Hero (1270×760)** | Town View 全景 + 三引擎 logo | 第一印象，必须抓眼球 |
| 2 | **GIF** | 三引擎并跑 Arena → diff 结果 | 核心差异化：no other tool does this |
| 3 | **Screenshot** | Memory Graph 力导向图 | "AI 的脑子可视化" |
| 4 | **GIF** | 插件安装：拖入文件夹 → 侧栏出现 → 使用 | 展示扩展性 |
| 5 | **Screenshot** | Artifact 预览面板（AI 生成 → 实时渲染） | 开发者最关心的 "看得见结果" |
| 6 | **Screenshot** | 设置页 Model Profiles + Ollama 配置 | 本地 AI 支持 |
| 7 | **Screenshot** | 对比表（KinetAios vs Claude Desktop vs Cursor vs Codex） | 竞品对比，一目了然 |

> Product Hunt Gallery 规则：首图必须是 1270×760，其余 1270×760 或同等比例。GIF 不超过 15 秒。

---

## 6. First Comment（可提前准备给朋友发）

> 这条由你自己发布后立即发，作为"第一条评论"带动讨论氛围：

Honestly the most surprising thing about building this was discovering that **memory is the hard part, not the model**.

Once you have three engines running side by side, the real question becomes: how does Claude know what Codex already figured out? How do you keep a consistent user profile across providers?

Our answer: automatic fact extraction after every turn → SQLite + FTS5 → inject on next session regardless of engine. Simple, no vector DB, fully transparent (there's a graph view to see everything the AI "knows" about you).

Curious — how are you all handling context across different AI tools? Are you just copy-pasting, or have you found something that works?

---

## 7. Twitter / X Launch Thread

**Tweet 1:**
We relaunch KinetAios on @ProductHunt today — now with 40+ new features including:

🏠 Gamified Town View
🧠 Memory Graph
⚔️ Arena (3-engine race)
🔌 Plugin System V2
🖥️ Ollama support

Three AI agents. One window. Zero cloud.

🧵👇 https://www.producthunt.com/posts/kinet-aios

[#1]

**Tweet 2:**
🏠 Town View: every project is a house, every conversation is a villager.

Watch agents work in real time on an isometric map. Remote machines float as cloud houses. Click any villager to jump to that conversation.

It's a genuine productivity interface — not a gimmick.

[#2]

**Tweet 3:**
🧠 The Memory Graph.

Every fact the AI extracted about you — visualized as a force-directed graph.

✅ Trace any memory to its source
✅ Conflicting memories flagged red
✅ Delete what's stale

Your AI's brain. Fully transparent.

[#3]

**Tweet 4:**
⚔️ Arena: same prompt → three engines racing in real time.

Claude Code. Codex. GLM/Kaios.

Side-by-side diff. AI judge. Token cost comparison.

Stop guessing which model is best for a task — watch them compete.

[#4]

**Tweet 5:**
🔌 Plugin System V2: drop a folder, your agent learns new skills.

Ships with:
📊 Office suite (Excel, PDF, OCR, 15 tools)
🎓 C++ learning module
📐 Math practice module

Custom tools, slash commands, system prompts, full-screen panels.

[#5]

**Tweet 6:**
🖥️ Now supports Ollama natively.

Auto-detects models. Native /api/chat endpoint. 32K context. Fully offline.

Plus: model profiles (save multiple configs, switch per conversation), Artifact preview, Visual Inspector, session goals, and more.

[#6]

**Tweet 7:**
Built with Electron + TypeScript. Vanilla TS renderer (no React). better-sqlite3 + FTS5. GPL v3. Windows + macOS.

No account. No telemetry. Your API key is the only credential.

👉 github.com/phinn/KinetAios
🚀 We're live on Product Hunt — come say hi!

[#7]

---

## 8. Reddit Title（r/devops, r/programming, r/LocalLLMs）

**KinetAios v1.5: I built an open-source desktop app that runs Claude Code, Codex, and Ollama side-by-side — with a gamified Town view, cross-engine memory graph, and a plugin system. 40+ features shipped since v1.**

---

## 9. Hacker News Title

**Show HN: KinetAios v1.5 – Desktop dashboard for Claude Code, Codex, and Ollama with cross-engine memory (GPL v3)**

---

## 10. LinkedIn Post

Excited to relaunch KinetAios on Product Hunt! 🚀

KinetAios is an open-source desktop app that runs three AI engines — Claude Code, Codex, and a built-in ReAct loop — from one window. No account, no cloud, no telemetry.

Since our first launch, we shipped 40+ features:

🏠 Town View — projects as pixel-art houses, conversations as villagers, remote machines as cloud houses
🧠 Memory Graph — visualize, trace, and edit everything the AI knows about you
⚔️ Arena — race three engines on one prompt, side-by-side diff, AI judge
🔌 Plugin System V2 — extensible with custom tools, commands, and panels
🖥️ Ollama native support — fully local, fully offline
🎨 Artifact preview, Visual Inspector, model profiles, and more

Built solo with Electron + TypeScript. Open source (GPL v3). Windows + macOS.

If you work with multiple AI engines, I'd love your feedback.

🔗 github.com/phinn/KinetAios

---

## 11. Key Differentiators（评论区快速参考）

1. **Three engines, one window** — Switch per-conversation without losing state. Claude, Codex, GLM/Kaios, Ollama.
2. **Cross-engine memory** — One user profile across all engines. Auto-extracted, editable, graphable.
3. **Town View** — Isometric gamified map. Projects = houses, agents = villagers. Not decorative — it's a real spatial interface.
4. **Arena** — Empirical engine comparison with diff + AI judging + cost tracking.
5. **Plugin System V2** — Custom tools, slash commands, system prompts, panels. Drag-and-drop install.
6. **Local-first** — All data in SQLite. No account. API key is the only auth.
7. **Ollama native** — Local models via `/api/chat`, no OpenAI compatibility shim needed.
8. **File snapshots + rollback** — Every write auto-snapshots. One-click revert.
9. **Pipeline** — Chain engines: Claude designs → Codex implements → GLM reviews.
10. **12 built-in tools** — shell, read/write/edit_file, grep, glob, git_diff, web_fetch, web_search, recall_memory, dispatch_agent.

---

## 12. What's New Since v1.2（详细 changelog，供评论区引用）

### Major Features
- **Town View** — Isometric pixel-art map of projects and conversations
- **Memory Graph** — Force-directed graph with memory sourcing + conflict detection
- **Arena Dashboard** — Multi-engine race with diff + AI judge + cost analytics
- **Plugin System V2** — Tools, slash commands, system prompts, full-screen panels
- **Artifact Preview** — Live HTML/SVG rendering in sandboxed iframe
- **Ollama Native Support** — Auto-detect, native endpoint, 32K context
- **Model Profiles** — Multiple LLM configs, per-conversation switching
- **Visual Inspector** — Circle-select web elements → AI modifies code
- **Session Goals** — `/goal` command to keep agents focused
- **Cross-session References** — `@conv:id` linking + session branching + DAG
- **Context Inspector** — View/edit the exact messages sent to the LLM
- **Global Search** — `Ctrl+Shift+F` across all conversations
- **Memory Sync** — Cross-device memory via MCP
- **Remote Agent Banner** — Real-time status bar for remote agent tasks
- **Web Search Tool** — Search engine integration for agents
- **Cost Dashboard** — Per-session, daily, monthly cost tracking + budget cutoffs

### Built-in Plugins
- **office-suite** — 15 tools (Excel, PDF, OCR, doc conversion) + 5 slash commands
- **cpp-learning** — C++ education module with 8 progressive levels
- **math-practice** — Math exercises for elementary students
- **low-altitude** — Low-altitude economy industry knowledge base
- **brainstorm** — Visual brainstorming with Excalidraw

---

## 13. Pre-Launch Checklist

### The Day Before
- [ ] 确认 Product Hunt 产品页信息已更新（tagline, description, gallery）
- [ ] 准备好 7 张 gallery 图/GIF（见 Section 5）
- [ ] GitHub README 更新到最新版本截图
- [ ] GitHub Release v1.5.0 已发布（带 release notes）
- [ ] 预约 Hunter（或自己 hunt）
- [ ] 通知 friends/supporters 准备 upvote + comment
- [ ] 测试安装包：Windows installer + macOS dmg 可正常安装

### Launch Day（PST 00:01）
- [ ] 正式 launch
- [ ] 立即发 Maker Comment（Section 4）
- [ ] 立即发 First Comment（Section 6）
- [ ] 发 Twitter thread（Section 7）
- [ ] 发 LinkedIn（Section 10）
- [ ] 提交 Hacker News（Section 9）
- [ ] 发 Reddit（Section 8）
- [ ] 通知所有 supporters
- [ ] 回复每一条 PH 评论（前 12 小时最关键）

### Launch Day +1
- [ ] 感谢所有支持者
- [ ] 在 GitHub 发布 launch 后总结
- [ ] 收集反馈，整理 issue
