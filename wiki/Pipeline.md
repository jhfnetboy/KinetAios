> 🌐 Language: **English** | [中文](Pipeline.zh-CN.md)

# Pipeline (Cross-Engine Orchestration)

A **pipeline** chains multiple stages, each specifying an engine + prompt. The previous stage's output auto-prepends to the next stage's prompt. This lets you, e.g., use Direct to analyze a problem, Claude Code to implement the fix, then Codex to review.

## Structure

```ts
type PipelineStage = {
  engine: EngineKind;  // 'direct' | 'claude-code' | 'codex'
  prompt: string;
  label?: string;      // optional display name
};

type Pipeline = {
  id: string;
  stages: PipelineStage[];
};
```

## How it works

```
Stage 1 (Direct)          Stage 2 (Claude Code)       Stage 3 (Codex)
  prompt: "Analyze..."      prev_output +               prev_output +
  → output A                prompt: "Implement..."      prompt: "Review..."
                             → output B                  → output C (final)
```

1. `TaskManager` creates a temporary conversation for each stage.
2. Each stage runs with its specified engine (switching clears cross-engine context).
3. The stage's final answer is captured and prepended to the next stage's prompt.
4. **2-minute per-stage timeout** with polling; if a stage exceeds the limit, the pipeline aborts.
5. **Fail-fast**: if any stage errors, the entire pipeline aborts and reports which stage failed.

## When to use

| Scenario | Pipeline |
|---|---|
| Analyze → Implement → Review | Direct → Claude Code → Codex |
| Draft → Refine → Test | Direct → Direct → Claude Code |
| Multi-model brainstorm | Direct (GLM) → Claude Code → Direct (DeepSeek) |

## Programmatic access

Pipelines are created via the TaskManager API (no UI yet — `ponytail:` MVP). The MCP server can trigger pipelines remotely.

## Key source files

- `src/main/TaskManager.ts` — `Pipeline` type, `runPipeline()`, stage timeout polling
- `src/shared/types.ts` — `PipelineStage`, `Pipeline` types
