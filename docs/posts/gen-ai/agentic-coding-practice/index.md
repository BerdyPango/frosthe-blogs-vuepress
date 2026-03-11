---
title: Agentic Coding 工具的指令、上下文与记忆系统对比
author: Frost He
excerpt: 对比 Claude Code、Cursor、Codex CLI 和 RooCode 在指令组织、记忆系统和扩展机制上的设计差异，并探讨跨工具迁移的实践策略
date: 2026-03-12
category:
- GenAI
tag:
- ai
- llms
- vibe-coding
---

[上一篇文章](../vibe-coding-agents/index.md)从 Benchmark 角度对比了各 Agent 与 LLM 的组合表现。但在日常开发中，**指令系统的灵活性**、**跨会话记忆能力**和**扩展机制的成熟度**往往比跑分更直接地决定体验。

本文聚焦 **Claude Code**、**Cursor**、**Codex CLI** 和 **RooCode**，对比以下维度：

1. 全局指令与项目指令
2. 上下文管理
3. 记忆系统与长期知识组织
4. 内置命令与扩展机制
5. 跨工具迁移与配置复用

---

## 一、全局指令与项目指令

指令文件的设计直接影响 Agent 对项目的理解深度和行为一致性。

### Claude Code：CLAUDE.md 分层体系

使用纯 Markdown 文件 `CLAUDE.md`，按目录层级自动加载：

| 层级 | 文件 | 作用域 |
|------|------|--------|
| 用户全局 | `~/.claude/CLAUDE.md` | 所有项目 |
| 项目共享 | `./CLAUDE.md` 或 `./.claude/CLAUDE.md` | 提交到 Git |
| 项目本地 | `./CLAUDE.local.md` | 不入版本控制 |

支持 `@` 语法引入外部文件（如 `@docs/coding-standards.md`），实现模块化。`/init` 自动生成初始文件。

### Cursor：.cursor/rules 分类规则

使用 `.mdc` 格式（YAML frontmatter + Markdown），核心特色是**四种激活模式**：

- **Always** — 每次对话加载
- **Auto Attached** — 通过 `globs` 匹配文件时自动加载
- **Agent Requested** — Agent 根据 `description` 自行判断
- **Manual** — 用户通过 `@ruleName` 显式引用

层级：Team Rules > Project Rules (`.cursor/rules/`) > User Rules > Legacy `.cursorrules`

### Codex CLI：AGENTS.md 逐级注入

使用 `AGENTS.md`，从 `~/.codex/` 全局到项目根目录再到当前目录逐级注入。支持 `AGENTS.override.md` 覆盖，合并上限 32 KiB。

### RooCode：按模式组织规则

独特之处是**按模式（Mode）组织规则目录**：`.roo/rules/`（通用）、`.roo/rules-code/`（Code 模式专属）、`.roo/rules-architect/`（Architect 模式专属）。文件按字母序追加到系统提示词。

### 对比

| 特性 | Claude Code | Cursor | Codex CLI | RooCode |
|------|------------|--------|-----------|---------|
| 格式 | 纯 Markdown | MDC (YAML + MD) | 纯 Markdown | 纯 Markdown |
| 指令文件 | `CLAUDE.md` | `.cursor/rules/*.mdc` | `AGENTS.md` | `.roo/rules[-mode]/*.md` |
| 激活策略 | 目录层级自动 | Always/Auto/Agent/Manual | 逐级遍历 | 按模式 + 字母序 |
| 模块化 | `@` 语法引入 | `@filename.mdc` 交叉引用 | 目录层级 | 符号链接 |
| 全局配置 | `~/.claude/CLAUDE.md` | Settings > Rules | `~/.codex/AGENTS.md` | `~/.roo/rules/` |

---

## 二、上下文管理

| 特性 | Claude Code | Cursor | Codex CLI | RooCode |
|------|------------|--------|-----------|---------|
| 压缩策略 | 自动（95%+ 触发）+ `/compact` | `/summarize` + 到限自动 | 无内置 | 可配阈值 + 手动 |
| 上下文扩展 | 子代理独立 200K 窗口 | Sub-agents 并行 + 语义索引 | 多代理分叉 + 会话恢复 | Boomerang Tasks |
| 上下文引用 | 自动 + 文件路径 | `@Files` `@Codebase` `@Docs` | 自动 | `@/path` + `@problems` |

核心差异：
- **CLI 工具**（Claude Code、Codex CLI）依赖自动压缩和子代理来扩展有效上下文
- **IDE 工具**（Cursor、RooCode）更多依赖语义索引和文件级引用精准注入
- Cursor 2.0 移除了 `@Web`、`@Git` 等手动引用，改由 Agent 自动收集——**上下文管理正从"用户手动组织"向"Agent 自主获取"演进**

---

## 三、记忆系统

跨会话记忆是提升长期协作效率的关键，各工具成熟度差异显著。

### 各工具的内置记忆

**Claude Code** — 双层体系：用户编写的 `CLAUDE.md` + Agent 自动积累的 Auto Memory（存储在 `~/.claude/projects/<project>/memory/MEMORY.md`，每次会话自动加载前 200 行）。用户可主动要求记忆（"记住始终使用 pnpm"），也可通过 `/memory` 管理。

**Cursor** — 自 v1.4 起内置 Memories，根据对话内容自动生成，存储在项目级别，Settings > Rules 中管理。社区补充方案包括 Cursor Memory Bank、MCP Knowledge Graph 等。

**Codex CLI** — 最精密的记忆系统。基于 SQLite 的两阶段管线：Phase 1 从会话提取原始记忆，Phase 2 由整合子代理更新 `MEMORY.md` 和可复用 Skills。自带记忆污染检测——使用网页搜索的会话会被标记并触发清理。

**RooCode** — 无内置记忆，依赖社区 MCP 方案（roo-code-memory-bank、roo-advanced-memory-bank），通过结构化 Markdown 文件跨会话存储。

| 特性 | Claude Code | Cursor | Codex CLI | RooCode |
|------|------------|--------|-----------|---------|
| 内置记忆 | Auto Memory | Memories | 两阶段管线 | 无（社区方案） |
| 存储 | Markdown | 项目级数据库 | SQLite + MD | MCP + MD |
| 自动/手动 | 自动 + 手动 | 纯自动 | 自动提取整合 | 方案依赖 |

### 项目长期知识的组织

Agent 的自动记忆偏向**操作层面**（构建命令、代码风格），而项目真正需要积累的**域知识**——架构决策、踩坑经验、领域模型——需要开发者主动组织。

#### 架构决策记录（ADR）

让 Agent 理解"为什么这样做"而非仅仅"现在是这样"：

```markdown
# ADR-003: 选择事件驱动架构替代同步 RPC

## 状态：已采纳（2026-01）
## 背景
订单服务与库存服务之间的同步调用导致级联故障...
## 决策
采用 Event-Driven Architecture，通过消息队列解耦服务间通信
## 后果
- 正面：服务隔离性提升，单点故障不再级联
- 负面：最终一致性增加了调试复杂度
- 约束：所有跨服务通信必须通过事件，禁止直接 RPC
```

#### 已知陷阱

Agent 反复犯同样的错是最常见的效率损耗，显式记录远比自动记忆可靠：

```markdown
## 数据库连接池
- 错误：Lambda 中每次创建新连接 → Aurora 连接数上限耗尽
- 正确：连接池复用，cold start 时初始化
```

#### 业务术语表

减少 Agent 对业务术语的误解：

```markdown
| 术语 | 定义 | 代码对应 |
|------|------|----------|
| 挂单 | 已创建未支付的订单 | `OrderStatus.PENDING` |
| 核销 | 优惠券使用和扣减 | `CouponService.redeem()` |
```

#### 推荐目录结构

```
docs/
├── adr/                    # 架构决策记录
├── pitfalls.md             # 已知陷阱
├── glossary.md             # 业务术语表
└── onboarding.md           # 项目上手指南
```

这些文件**不依赖任何特定 Agent 工具**，通过各工具的引用机制纳入上下文即可（Claude Code 用 `@docs/adr/`、Cursor 用 Always 规则引用、Codex 在 AGENTS.md 中声明路径、RooCode 放入 `.roo/rules/`）。既服务于 Agent，也服务于团队 onboarding。

---

## 四、内置命令与扩展机制

### 内置 Slash Commands

| 能力 | Claude Code | Cursor | Codex CLI | RooCode |
|------|------------|--------|-----------|---------|
| 上下文管理 | `/compact` `/context` `/clear` | `/summarize` | `/clear` | 手动按钮 |
| 项目初始化 | `/init` | `/Generate Cursor Rules` | `/init` | - |
| 代码审查 | `/review` | - | `/review` | - |
| 模型切换 | `/model` | 设置面板 | `/model` | 设置面板 |
| 记忆管理 | `/memory` | Settings 面板 | - | - |
| 权限/模式 | - | - | `/permissions` `/plan` | `/code` `/architect` `/ask` `/debug` |

### 自定义命令与 Skills

四款工具都支持自定义 Slash Commands，格式都是 Markdown，存放位置不同：

| 工具 | 自定义命令路径 | Skills |
|------|---------------|--------|
| Claude Code | `.claude/commands/` | `.claude/skills/<name>/SKILL.md`，遵循 [Agent Skills 开放标准](https://agentskills.io) |
| Cursor | `.cursor/commands/` | 无独立 Skills 概念 |
| Codex CLI | 通过 Skills 系统 | `.agents/skills/<name>/SKILL.md`，`$skill-name` 调用 |
| RooCode | `.roo/commands/` | 无独立 Skills，通过自定义 Modes 实现 |

### 各工具独有的扩展能力

- **Claude Code** 拥有最丰富的扩展生态：12 个生命周期 **Hooks**（PreToolUse 可阻止操作、PostToolUse 自动格式化等）、自定义子代理（`.claude/agents/`）、细粒度工具权限（`allowedTools/blockedTools`）
- **Cursor** 侧重 Agent 模式增强：多 Agent 并行（Mission Control）、Background Agents、YOLO Mode
- **Codex CLI** 的 Skills 支持渐进式披露（先加载元数据，调用时才加载完整指令），多代理通过 `config.toml` 配置
- **RooCode** 的**自定义模式**最具特色：通过 `.roomodes` 定义角色、工具权限（含 `fileRegex` 限制），Orchestrator 模式支持 Boomerang Tasks 编排子任务

---

## 五、跨工具迁移与配置复用

个人开发者经常在多款工具间切换——IDE 中用 Cursor 快速编辑，终端用 Claude Code 做重构，偶尔用 Codex CLI 跑沙箱实验。如果配置不通用，迁移成本将非常高。

目前**不存在完美的通用标准**，但可以通过以下策略显著降低成本。

### 策略一：纯 Markdown 作为配置基底

四款工具的最大公约数是 Markdown——Claude Code、Codex CLI、RooCode 原生使用 `.md`，Cursor 的 `.mdc` 去掉 frontmatter 也是 Markdown。

实践：**维护一份工具无关的 `docs/ai-instructions.md` 作为通用指令源**，各工具通过自身的引用机制桥接：

- Claude Code：`CLAUDE.md` 中 `@docs/ai-instructions.md`
- Cursor：`alwaysApply: true` 的规则引用
- Codex CLI：`AGENTS.md` 中声明路径
- RooCode：`.roo/rules/` 下符号链接

### 策略二：AGENTS.md 作为兼容层

`AGENTS.md` 正成为**事实上的跨工具兼容格式**：Codex CLI 原生支持，Cursor 2.0 起可读取，Claude Code 通过 `@AGENTS.md` 引入，RooCode 可符号链接。

建议：**团队共享的通用指令写在 `AGENTS.md`，工具特定配置保留在各自目录**。

### 策略三：MCP 配置统一

MCP（Model Context Protocol）已成为 Agent 连接外部服务的事实标准，四款工具均已支持。配置文件路径不同（`.mcp.json` / `.cursor/mcp.json` / `.roo/mcp.json`），但**结构几乎一致**（`mcpServers` 对象，每个 server 包含 `command`、`args`、`env`）。维护一份模板，通过脚本同步到各目录即可。

### 策略四：Commands 的有限复用

自定义 Slash Commands 的核心都是 Markdown prompt，只是存放位置和 frontmatter 不同。对于简单命令（`/review`、`/refactor`），**prompt 内容可直接复用**，在各目录下创建薄包装层即可。

### 推荐的项目结构

```
project-root/
├── AGENTS.md                    # 通用指令（Codex 原生 / Cursor 兼容）
├── CLAUDE.md                    # Claude Code 入口（@AGENTS.md + 特定配置）
├── docs/
│   ├── ai-instructions.md       # 编码规范（工具无关）
│   ├── adr/                     # 架构决策
│   ├── pitfalls.md              # 犯错经验
│   └── glossary.md              # 业务术语
├── .claude/                     # Claude Code 特定
│   ├── commands/
│   └── skills/
├── .cursor/                     # Cursor 特定
│   ├── rules/
│   └── mcp.json
├── .roo/                        # RooCode 特定
│   ├── rules/
│   └── mcp.json
└── .mcp.json                    # Claude Code MCP（与其他目录结构一致）
```

核心原则：**知识放在 `docs/`（工具无关），配置放在 `.claude/` `.cursor/` `.roo/`（工具特定），通过引用连接两者**。无论工具如何更迭，知识资产始终留存在项目中。

---

## 总结

四款工具体现了不同的产品哲学：

| | 核心理念 | 最强项 |
|---|---------|--------|
| **Claude Code** | 纯 Markdown + 约定优于配置 | 扩展生态最完整（Skills + Hooks + 子代理） |
| **Cursor** | 可视化 IDE 体验 | 上下文管理最直观（语义索引 + @-mentions） |
| **Codex CLI** | 工程化 + 安全优先 | 记忆系统最精密（两阶段管线 + 污染检测） |
| **RooCode** | 角色化分工 | 行为控制最细粒度（模式 + 工具权限） |

对于在多款工具间切换的开发者，核心建议是：**将项目知识沉淀为工具无关的文档，将工具配置视为薄包装层**。这样无论工具如何更迭，知识资产始终留存。
