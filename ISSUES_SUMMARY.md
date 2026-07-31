# KinetAios 问题点总结

> 基于 git 历史 116 个 fix commit + 当前对话反馈，按类别归纳。

---

## 一、CSS / 布局问题（最高频，~30 次）

### 1.1 设置页 tab 切换宽度跳变（7 次反复修复！）
**这是整个项目修复次数最多的单一问题，折腾了 7 轮才真正解决。**

| 次数 | Commit | 尝试的方案 |
|------|--------|-----------|
| 1 | `204ef60` | fadeIn → panelIn (opacity+translateY) |
| 2 | `468263e` | 彻底修复（声称） |
| 3 | `f722643` | 移除强制 reflow + opacity 动画（Chromium 崩溃） |
| 4 | `060008d` | 滚动条出现/消失导致宽度变化 |
| 5 | `11ab797` | 根因：双层 overflow-y:auto 竞争 |
| 6 | `9740e3d` | 多层防御 |
| 7 | `0130e21` | 真正根因：自定义滚动条 |

**根因本质**：`#settings-view` 和 `#settings` 两层都设了 `overflow-y: auto`，滚动条空间预留不一致。

### 1.2 文字选择不全（3 次）
- `53cc6bc` textarea 和消息气泡文字选不全
- `7c3dc3a` 文件 tab 代码编辑器文字选不全
- `032deff` markdown 内容跨元素选择不全

### 1.3 侧边栏频道文字居中（3 次）
- `da8ca98` → `5b807f2` → `33f43a9`，折腾 3 轮才找到根因（flex-direction 继承）

### 1.4 插件面板高度问题（2 次）
- `dd0a484` 插件面板高度撑满
- `17b09be` 插件面板切回对话高度减半

### 1.5 Town View 布局问题（5 次）
- `e6e1f4d` town-view display:flex 覆盖 .view{display:none} 导致 chat 区域被压缩一半
- `0ba1125` → `ce5cda9` → `0a23aa9` → `3e0f421` town panel 定位反复调整（4 轮）

### 1.6 主题相关可见性（4 次）
- `ae4769b` Craft 角色装饰不可见 — body 改到 #main
- `599a716` SEED 聊天区机甲装饰可见度
- `973e8c7` Git 工具栏图标在主题下渲染为点
- `7ccb518` 淡色主题在独立窗口未生效

### 1.7 其他 CSS
- `465f87a` 设置按钮被遮挡（6 按钮挤溢出）
- `ac9f04e` 关系标签文字竖排
- `e4351ba` 上下文检查器挤压成横线
- `0677d9d` 上下文检查器显示横线

---

## 二、插件系统问题（~18 次）

**插件系统是 bug 最多的子系统，从加载到渲染几乎每个环节都出过问题。**

### 2.1 插件加载 / 注册
- `0191e51` manifest.json → plugin.json（文件名不一致）
- `926e2b2` 插件列表缺少 brainstorm — category 未注册
- `b26e820` 插件列表不显示 education 分类
- `b3276aa` 开发模式下从源码加载路径
- `960a12b` 关闭插件后侧栏菜单不清除

### 2.2 插件面板渲染
- `d8e3337` CSP 阻止脚本执行 → 改用 iframe
- `dd0a484` Excalidraw CDN 加载失败
- `17b09be` 切回对话高度减半
- `37a586f` brainstorm panel 语法错误 + loading 卡住

### 2.3 brainstorm 插件专项（5 次）
- `f994868` 轮询字段名错误 → 永久 timeout
- `6cfb098` 轮询改事件推送
- `7f5c08a` addToCanvas 清空画布
- `187becc` 添加元素看不见
- `37a586f` 语法错误 + loading 卡住

### 2.4 其他
- `7f721d5` 插件系统全面修复
- `5ea57ca` 图标尺寸不统一
- `ec18017` tool_call arguments JSON 被截断

---

## 三、截图 / 语音 / 媒体问题（~10 次）

### 3.1 截图
- `8dc9cee` 截图空白图
- `5af3f53` 改 getDisplayMedia
- `876a9bd` 截图权限提示
- `3273ddb` CSP 阻止 base64 图片
- `eb97c85` overlay 遮挡输入框
- `01e4135` 截图后输入框消失

### 3.2 语音
- `876a9bd` 麦克风权限没拿
- `412bed8` Web Speech API → VAD 方案

### 3.3 预览 iframe
- `ab3f22f` iframe alert 弹窗
- `fe57207` 静态 iframe 也弹 alert
- `e78b1a2` 预览面板无法显示
- `3fbd1c0` webview 预览显示源码
- `9c7e310` 切换文件时 webview 不刷新
- `00201f1` 选 HTML 文件预览和编辑都不显示

---

## 四、模型 / API 配置问题（~8 次）

- `cfd9804` Ollama 默认 4096 ctx 太小 → 400
- `78be0cd` Ollama 超长上下文 → 400
- `c78643f` Ollama 模型选择 datalist 问题
- `7f41ebe` 模型配置档重启后丢失
- `1ab25d2` model-input 和 profile-select 同时显示竞态
- `3be628b` 余额查询不区分 Coding Plan
- `17b3218` Coding Plan 用量端点错误
- `d28b507` Coding Plan 用量查询端点修正

---

## 五、安全 / 健壮性问题（~6 次）

- `93d560b` SSRF/timing-attack/listener-leak/persistence
- `133b037` P0+P1+P2 安全漏洞
- `a25185a` SSRF/shell-open/正则注入/argument injection
- `19669a1` 安全加固与类型修复
- `2abd991` 二进制检测/栈溢出/cost重复/孤儿数据/命令注入
- `2fefd18` office-suite 安全加固
- `05c7bec` read_file OOM 崩溃

---

## 六、CI / 打包问题（~5 次）

- `fbc76c0` npmmirror 在 GH Actions 超时
- `d5f40d7` electron-builder 没 GH_TOKEN 必挂
- `a97d044` GH Actions 缺 contents:write 权限
- `e3f4d6e` asar 打包在其他 Windows 报「损坏」
- `7ccb518` 淡色主题在独立窗口未生效

---

## 七、功能交互 / 逻辑问题（~10 次）

- `6a29b69` 远程任务按钮点击无反应
- `8dad221` Town 远程节点始终显示离线
- `79d10bc` 记忆图谱点击节点无反应
- `fe22216` 记忆图谱 detail 面板空状态折叠为 0
- `b67995c` 中断后连续对话上下文断裂
- `4f04906` showChat() 遗漏 active 移除
- `13d6655` /goal 命令在聊天区显示反馈
- `4e941f6` /goal 加入斜杠命令菜单
- `5077944` clipboard copy 在 contextIsolation 下失效
- `cafd6f2` DevTools 快捷键被清空菜单覆盖

---

## 八、Visual Inspector 问题（4 次，全部重构）

- `ea192d2` 不响应点击 → 改用轮询
- `8c37a18` 彻底重构 — overlay 移到 renderer
- `301d199` 绕开主进程 IPC
- `e6c0f64` ESC 取消 + AI 无响应

---

## 九、i18n 国际化问题（~4 次）

- `a9d49c5` common.ok 缺 zh-CN/en/zh-TW
- `afcb461` 上下文检查器按钮 i18n 缺失
- `28aca37` CSS 变量名错误
- `2abd991` i18n 缺失

---

## 十、核心模式总结

### 反复修复同一问题（Top 5）

| 问题 | 修复次数 | 根因类型 |
|------|---------|---------|
| 设置页 tab 切换宽度跳变 | **7 次** | 双层 overflow 竞争 + 滚动条 gutter |
| 侧边栏频道文字居中 | **3 次** | flex-direction 继承 |
| Town panel 定位 | **5 次** | flex 布局 + fixed 定位干扰 |
| brainstorm 插件 | **5 次** | 轮询字段 + schema + CDN |
| Visual Inspector | **4 次** | IPC 通信架构 |
| 文字选择不全 | **3 次** | user-select / flex 布局 |
| Ollama 400 错误 | **2 次** | ctx 大小 + 端点选择 |

### 问题模式归类

1. **CSS 布局知识不足**（30+ 次）：flex 嵌套、overflow 嵌套、position:fixed 与 flex 的交互、scrollbar-gutter 等。最高频的问题类型。
2. **先做后测**（多次）：声称"彻底修复"后下个 commit 又修同一个问题，说明没有验证。
3. **跨进程通信架构**（~8 次）：Visual Inspector、brainstorm 插件、远程任务都因 IPC 通信方案反复重构。
4. **安全漏洞补丁式修复**（6 次）：每次审查都发现新的 SSRF/注入/泄漏，说明初始设计缺乏安全考量。
5. **配置 / 状态持久化**（~8 次）：profileId 丢失、竞态、配置档切换等问题。

### 建议改进方向

1. **CSS 布局**：建立一套标准的"滚动容器"模式，只用一层 `overflow-y: auto` + `scrollbar-gutter: stable`，避免嵌套。
2. **验证流程**：每次 fix 后在 app 中实际操作验证，不要声称"彻底修复"后下个 commit 又修。
3. **插件架构**：统一插件加载/渲染/通信规范，避免每个插件都踩一遍坑。
4. **安全设计**：在功能开发初期就考虑 SSRF/注入/CSP，而非事后补丁。
