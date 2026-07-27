> 🌐 Language: [English](Pipeline.md) | **中文**

# Pipeline(跨引擎编排)

**Pipeline** 串联多个 stage,每个指定引擎 + prompt。上一 stage 的输出自动拼到下一 stage 的 prompt 前面。比如用 Direct 分析问题、Claude Code 实现修复、Codex 做 review。

## 结构

```ts
type PipelineStage = {
  engine: EngineKind;  // 'direct' | 'claude-code' | 'codex'
  prompt: string;
  label?: string;      // 可选显示名称
};

type Pipeline = {
  id: string;
  stages: PipelineStage[];
};
```

## 工作原理

```
Stage 1 (Direct)          Stage 2 (Claude Code)       Stage 3 (Codex)
  prompt: "分析..."          上一步输出 +                 上一步输出 +
  → 输出 A                   prompt: "实现..."           prompt: "审查..."
                             → 输出 B                    → 输出 C(最终)
```

1. `TaskManager` 为每个 stage 创建临时会话。
2. 每个 stage 用指定引擎运行(切换清跨引擎上下文)。
3. stage 的最终回答被捕获,拼到下一 stage 的 prompt 前面。
4. **2 分钟/stage 超时**轮询;如果 stage 超时,pipeline 中止。
5. **失败即中止**:任何 stage 报错,整个 pipeline 中止并报告哪个 stage 失败。

## 什么时候用

| 场景 | Pipeline |
|---|---|
| 分析 → 实现 → 审查 | Direct → Claude Code → Codex |
| 草稿 → 精修 → 测试 | Direct → Direct → Claude Code |
| 多模型头脑风暴 | Direct (GLM) → Claude Code → Direct (DeepSeek) |

## 编程式调用

Pipeline 通过 TaskManager API 创建(暂无 UI —— `ponytail:` MVP)。MCP server 可远程触发。

## 关键源文件

- `src/main/TaskManager.ts` —— `Pipeline` 类型、`runPipeline()`、stage 超时轮询
- `src/shared/types.ts` —— `PipelineStage`、`Pipeline` 类型
