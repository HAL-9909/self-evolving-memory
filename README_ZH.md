# openclaw-self-evolving-memory (中文版)

> 让你的 AI Agent 拥有真正可进化、自动维护的长期记忆系统。

这是一个适用于 [OpenClaw](https://github.com/openclaw/openclaw) 的多层记忆管理 Skill。它不仅能帮你的 Agent 记住信息，还能将那些零散的对话印象转化为**结构化、可自我维护的持久化记忆**，从根本上解决 Agent 总是“健忘”或重复犯错的问题。

---

## 核心优势

AI Agent 常见的记忆痛点，这个系统都能解决：

- **告别会话失忆**：通过 `SESSION-STATE.md` 实现复杂多步骤任务的跨上下文记忆。
- **防止记忆腐烂**：自动将转瞬即逝的日常记录与稳定的长期记忆分层处理，告别“记忆乱堆”。
- **强化行为改变**：不仅是存储信息，更通过“强化层”（SOUL/AGENTS/TOOLS）将反复出现的反馈转化为 Agent 的行为准则。
- **分层架构**：六层记忆结构（热态 → 日常 → 长期 → 事实 → 总结 → 检索），确保 Agent 在合适的时间获取正确的信息。
- **极致简洁**：根目录 `MEMORY.md` 仅作为精简摘要，保持上下文环境清爽。

**核心逻辑：记忆不是存储问题，而是路由与执行问题。**

---

## 架构概览

```
第0层：强化层 (Enforcement)     — SOUL.md / AGENTS.md / TOOLS.md
         ↑ 针对反复发生的问题，将其固化为永久的行为准则

第1层：热态 (Hot State)         — SESSION-STATE.md
         ↑ 当前任务、阻塞点、交接信息 — 任务结束即清理

第2层：日常工作记忆 (Daily)      — memory/YYYY-MM-DD.md
         ↑ 纠错、执行结果、调试日志、短期反思

第3层：结构化长期记忆 (Long-term) — memory/preferences.md  system.md  projects.md
         ↑ 经确认的稳定事实、用户偏好、项目决策

第4层：根级摘要 (Root Summary)   — MEMORY.md
         ↑ 每次会话自动注入，保持精简，只存高价值摘要

第5层：语义检索 (Semantic Recall) — memory_search + 向量索引
         ↑ 仅作为辅助查询层，而非真理源
```

---

## 日常维护与自检

为了防止记忆系统“生锈”，请定期执行：

- **状态清理**：`SESSION-STATE.md` 是否还滞留在已结束的任务中？
- **根目录瘦身**：`MEMORY.md` 是否太长了？将其详细内容移至 `memory/*.md`。
- **沉淀推广**：检查 `memory/YYYY-MM-DD.md` 中是否有已确认的长期事实或偏好，将其提升到结构化记忆层。
- **事实审计**：检查 `memory/system.md`，清理过时的环境信息。
- **强化审计**：发现反复出现的问题了吗？将其硬编码到 SOUL/AGENTS/TOOLS 中。

建议配置 `HEARTBEAT.md` 进行定期自检。

---

## 兼容与适配

- **OpenClaw**: 2026.3.x 及更高版本
- **大模型**: 适配所有主流模型 (Claude, GPT, Gemini, 本地模型等)
- **向量检索**: 可选但推荐；支持 Ollama, OpenAI 及其兼容 API

---

## 安装部署

```bash
# 将 skill 复制到你的 OpenClaw skills 目录
cp -r openclaw-self-evolving-memory ~/.openclaw/skills/self-evolving-memory
```

---

## 快速配置

运行初始化脚本，即可自动完成配置：

```bash
cd ~/.openclaw/skills/self-evolving-memory
bash scripts/setup.sh
```

**脚本功能：**
- 自动检测并选择你的工作区
- 复制所有记忆文件模板（不会覆盖已存在的）
- 检查必要配置文件（SOUL.md 等）并提示缺失
- 检查向量检索配置并引导优化

**首次验证：**
安装后，直接问你的 Agent：
> “检查一下我的记忆系统设置，看看有没有缺什么。”

---

## 原理与路由

当信息产生时，系统自动完成路由：

| 信号/场景 | 路由目的地 |
|--------|------------|
| 用户说“记住这个” | 日常记录 → 稳定后移入结构化记忆 |
| 任务开始/上下文变更 | `SESSION-STATE.md` |
| 发现错误/纠错 | 日常记录 → 达到阈值后进入强化层 |
| 发现新的环境事实 | 日常记录 → `memory/system.md` |
| 做出项目决策 | 日常记录 → `memory/projects.md` |
| 发现用户偏好 | 日常记录 → `memory/preferences.md` |
| 遇到反复出现的麻烦(2次+) | 直接硬编码进 SOUL/AGENTS/TOOLS |

---

## 文件引用说明

- `SKILL.md`: 主逻辑定义
- `references/`: 
  - `runtime-protocol.md` (运行规范)
  - `embedding-setup.md` (向量搜索配置)
  - `setup-checklist.md` (手动配置手册)

## 开源协议

MIT
