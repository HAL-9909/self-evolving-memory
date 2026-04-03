<p align="right">
  <a href="#english-version">English</a> | <a href="#中文版本">中文</a>
</p>

---

<a id="english-version"></a>

# openclaw-self-evolving-memory

> Your AI agent finally has a memory system that actually works — and gets better over time.

A layered memory skill for [OpenClaw](https://github.com/openclaw/openclaw) that transforms agent memory from fragile session notes into a **structured, self-maintaining, behavior-changing long-term memory system**.

---

## Core Advantages

Most AI agent memory systems fail in the same ways. This one doesn't.

**❌ The problem with every other approach:**
- The agent "remembers" in a session, then forgets everything the next day
- Vector search finds the right fragment, but the agent still makes the same mistake
- Preferences, errors, decisions, and random notes pile into one giant undifferentiated file
- You've said the same correction five times and it still doesn't stick
- Nobody knows which memory file is the real source of truth anymore

**✅ What self-evolving-memory does differently:**

| Problem | How this solves it |
|--------|-------------------|
| Cross-session amnesia | Hot state layer captures task continuity across context windows |
| "Finds but doesn't change" | Enforcement layer promotes patterns into hard rules (SOUL/AGENTS/TOOLS) |
| Memory soup | Six-layer architecture routes every piece of information to the right place |
| Recurring mistakes | 2-strike rule: if it happens twice, it gets hardened into a behavioral rule |
| Bloated context | Root `MEMORY.md` stays short; detail lives in structured sub-files |

**The key insight:** Memory isn't a storage problem. It's an information routing problem. This system routes correctly — and then enforces.

---

## Architecture Overview

Six layers, one formal ledger. No fragmentation.

```
Layer 0  Enforcement     SOUL.md / AGENTS.md / TOOLS.md / hooks
         ↑ harden recurring issues into permanent behavioral rules

Layer 1  Hot State       SESSION-STATE.md
         ↑ current task, blockers, handoff — cleared after each task

Layer 2  Daily Memory    memory/YYYY-MM-DD.md
         ↑ corrections, outcomes, debugging notes, short reflections

Layer 3  Long-term       memory/preferences.md  system.md  projects.md
         ↑ stable facts, promoted from daily when confirmed

Layer 4  Root Summary    MEMORY.md
         ↑ auto-injected every session — kept ruthlessly short

Layer 5  Semantic Recall memory_search + embedding index
         ↑ retrieval layer only — never the source of truth
```

**Promotion flows upward. Truth lives at Layer 3–4. Behavior changes happen at Layer 0.**

---

## How It Works

### Memory routing

Every piece of information gets routed to the right layer automatically:

| Signal | Destination |
|--------|------------|
| "Remember this" / new preference stated | Daily → `preferences.md` if stable |
| Task starts or context changes | `SESSION-STATE.md` |
| Error found / correction made | Daily → enforcement layer if recurring |
| Stable environment fact discovered | Daily → `memory/system.md` |
| Project decision made | Daily → `memory/projects.md` |
| Recurring annoyance (2+ times) | Hardened into SOUL / AGENTS / TOOLS |

### Promotion state machine

```
observed  →  logged in SESSION-STATE.md or daily
curated   →  promoted into structured long-term memory
hardened  →  encoded into enforcement layer (permanent behavior change)
stable    →  validated, remains until explicitly marked stale
```

**The 2-strike rule:** If the same problem appears twice, stop logging it. Harden it.

### Task closeout protocol

1. Reset `SESSION-STATE.md` — clear the hot state
2. Write daily recap — what happened, what was learned
3. Promote anything stable — to preferences, system, or projects
4. Enforce anything recurring — update SOUL/AGENTS/TOOLS
5. Mark converged entries — annotate old daily files if content has been promoted

---

## Regular Maintenance & Self-Checks

- **Hot state stale?** — Is `SESSION-STATE.md` describing a task that ended days ago?
- **Root bloat?** — Is `MEMORY.md` growing into a journal? It should be ≤ 15 lines.
- **Promotion backlog?** — Are there daily entries that should be in `preferences.md` or `system.md`?
- **Stale facts?** — Is `memory/system.md` referencing endpoints or paths that no longer exist?
- **Hardening gap?** — Are there recurring issues that only exist in daily logs but haven't reached the enforcement layer?

Configure `HEARTBEAT.md` in your workspace to make the agent self-audit on a schedule.

---

## Compatibility

- **OpenClaw**: 2026.3.x and later
- **Models**: Any model (Claude, GPT, Gemini, local models, etc.)
- **Embedding**: Optional but recommended; supports Ollama, OpenAI, and any OpenAI-compatible API
- **OS**: Linux, macOS, any environment running OpenClaw

---

## Installation

```bash
cp -r openclaw-self-evolving-memory ~/.openclaw/skills/self-evolving-memory
```

## Quick Setup

```bash
cd ~/.openclaw/skills/self-evolving-memory
bash scripts/setup.sh
```

The script detects your workspace, copies all memory file templates, and checks your configuration. See `references/setup-checklist.md` for manual setup.

Ask your agent to verify:
> "Check my memory system setup and tell me if anything is missing."

---

## File Reference

```
SKILL.md
scripts/
  setup.sh
templates/
  SESSION-STATE.md
  MEMORY.md
  HEARTBEAT.md
  memory/
    preferences.md  system.md  projects.md  MEMORY.md
references/
  runtime-protocol.md
  embedding-setup.md
  setup-checklist.md
```

---

## Credits

Inspired by [Anthropic's agent best practices](https://docs.anthropic.com/en/docs/build-with-claude/agentic) and real-world multi-agent OpenClaw deployments.

## License

MIT

---

---

<a id="中文版本"></a>

# openclaw-self-evolving-memory（中文版）

> 让你的 AI Agent 拥有真正可进化、自动维护的长期记忆系统。

适用于 [OpenClaw](https://github.com/openclaw/openclaw) 的多层记忆管理 Skill，将零散的对话印象转化为**结构化、可自我维护的持久化记忆**，从根本上解决 Agent 总是"健忘"或重复犯错的问题。

---

## 核心优势

AI Agent 常见的记忆痛点，这个系统都能解决：

**❌ 其他方案的通病：**
- 新会话开始，Agent 对上次的事一无所知
- 向量搜索找到了记忆，但 Agent 还是按老方式做事
- 偏好、错误、决策、日志全混在一个文件里
- 同一个错误纠正了五次，第六次还是老样子
- 没人知道哪个记忆文件是最终的真相

**✅ self-evolving-memory 的不同之处：**

| 问题 | 解决方式 |
|--------|----------|
| 跨会话失忆 | 热态层捕获任务连续性，跨上下文窗口不丢失 |
| "找到但不改变" | 强化层将模式升格为硬规则（SOUL/AGENTS/TOOLS） |
| 记忆乱堆 | 六层架构自动路由，每条信息都有归宿 |
| 反复犯错 | 两次法则：同一问题出现两次，直接固化为行为规则 |
| 上下文膨胀 | 根级 `MEMORY.md` 保持精简，细节存入结构化子文件 |

**核心逻辑：记忆不是存储问题，而是路由与执行问题。**

---

## 架构概览

六层架构，一个正式账本，不碎片化。

```
第 0 层  强化层        SOUL.md / AGENTS.md / TOOLS.md
         ↑ 反复出现的问题 → 固化为永久行为准则

第 1 层  热态          SESSION-STATE.md
         ↑ 当前任务、阻塞点、交接信息 — 任务结束即清理

第 2 层  日常工作记忆   memory/YYYY-MM-DD.md
         ↑ 纠错记录、执行结果、短期反思

第 3 层  结构化长期记忆  memory/preferences.md  system.md  projects.md
         ↑ 经确认的稳定事实，从日常记忆升格而来

第 4 层  根级摘要       MEMORY.md
         ↑ 每次会话自动注入，保持精简

第 5 层  语义检索       memory_search + 向量索引
         ↑ 仅作辅助查询层，非真理源
```

**信息向上升格。真相住在第 3–4 层。行为改变发生在第 0 层。**

---

## 原理与路由

每条信息都自动路由到正确的层：

| 信号/场景 | 路由目的地 |
|--------|------------|
| 用户说"记住这个" | 日常记录 → 稳定后移入 preferences.md |
| 任务开始/上下文变更 | SESSION-STATE.md |
| 发现错误/纠错 | 日常记录 → 达到阈值后进入强化层 |
| 发现新的环境事实 | 日常记录 → memory/system.md |
| 做出项目决策 | 日常记录 → memory/projects.md |
| 遇到反复出现的麻烦（2次+） | 直接硬编码进 SOUL/AGENTS/TOOLS |

**两次法则：** 同一问题出现两次，不要再记日志了，直接把它变成规则。

### 任务收口协议

1. 重置 `SESSION-STATE.md`，清理热态
2. 写日常复盘到 `memory/YYYY-MM-DD.md`
3. 升格稳定内容至结构化记忆
4. 反复问题写进强化层
5. 标注旧 daily 文件中已收敛的条目

---

## 日常维护与自检

- **热态过期？** `SESSION-STATE.md` 是否还在描述几天前结束的任务？
- **根目录膨胀？** `MEMORY.md` 是否超过 15 行？将细节移至 `memory/*.md`。
- **升格积压？** 有没有 daily 里已确认但还未升格的内容？
- **事实过时？** `memory/system.md` 是否有失效的路径或配置？
- **强化缺口？** 有没有反复出现的问题只在日志里，还没进强化层？

在工作区配置 `HEARTBEAT.md` 可让 Agent 定期自检。

---

## 兼容与适配

- **OpenClaw**: 2026.3.x 及更高版本
- **大模型**: 适配所有主流模型（Claude, GPT, Gemini, 本地模型等）
- **向量检索**: 可选但推荐；支持 Ollama, OpenAI 及其兼容 API
- **操作系统**: Linux, macOS, 任何运行 OpenClaw 的环境

---

## 安装部署

```bash
cp -r openclaw-self-evolving-memory ~/.openclaw/skills/self-evolving-memory
```

## 快速配置

```bash
cd ~/.openclaw/skills/self-evolving-memory
bash scripts/setup.sh
```

脚本会自动检测工作区、复制所有记忆文件模板、检查配置完整性。详见 `references/setup-checklist.md`。

安装后验证：
> 问你的 Agent："检查一下我的记忆系统设置，看看有没有缺什么。"

---

## 文件结构

```
SKILL.md
scripts/
  setup.sh
templates/
  SESSION-STATE.md
  MEMORY.md
  HEARTBEAT.md
  memory/
    preferences.md  system.md  projects.md  MEMORY.md
references/
  runtime-protocol.md
  embedding-setup.md
  setup-checklist.md
```

---

## 开源协议

MIT
