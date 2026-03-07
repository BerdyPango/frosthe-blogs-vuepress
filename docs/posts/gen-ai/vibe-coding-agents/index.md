---
title: Vibe Coding Agents 组合及比较
author: Frost He
excerpt: 通过比较市面上常见的 Agents 及 LLMs 的适配度，提供一个组合选择的参考
date: 2026-03-07
category:
- GenAI
tag:
- ai
- llms
- vibe-coding
---

## Vibe Coding Agent

目前市面上主流的 Vibe Coding Agent 工具如下：

| Agent                                                                                     | 提供方              | 简介                                                                             |
| ----------------------------------------------------------------------------------------- | ------------------- | -------------------------------------------------------------------------------- |
| [Claude Code](https://claude.ai/code)                                                     | Anthropic           | Anthropic 官方 CLI，深度集成 Claude 模型，支持文件读写、终端操作等全流程代码任务 |
| [Codex CLI](https://github.com/openai/codex)                                              | OpenAI              | OpenAI 官方命令行 Agent，基于 GPT 系列模型，支持 Medium/High 两档上下文模式      |
| [GitHub Copilot](https://github.com/features/copilot)                                     | GitHub / Microsoft  | 集成于 VS Code、JetBrains 等主流 IDE，支持代码补全、对话式编程及 Agent 模式      |
| [Cursor](https://cursor.com)                                                              | Anysphere           | 基于 VS Code 的 AI 原生编辑器，内置多模型切换和 Composer Agent 模式              |
| [Windsurf](https://codeium.com/windsurf)                                                  | Codeium             | Codeium 出品的 AI IDE，具备 Cascade 工作流引擎，支持多步骤自主任务               |
| [Zed AI](https://zed.dev)                                                                 | Zed Industries      | 高性能现代编辑器，国内版与智谱 GLM 深度合作，支持 Assistant 及 Agent 模式        |
| [RooCode](https://marketplace.visualstudio.com/items?itemName=RooVeterinaryInc.roo-cline) | Roo Veterinary Inc. | VS Code 插件，fork 自 Cline，支持自定义模型和 Agent 行为模式（Auto/Med/High）    |
| [opencode](https://github.com/sst/opencode)                                               | SST                 | 开源命令行 Agent，Terminal UI 风格，支持多种 LLM 后端                            |
| [FactoryCLI](https://factory.ai)                                                          | Factory             | 面向工程团队的 CLI Agent，强调代码质量和上下文管理                               |
| [Warp](https://www.warp.dev)                                                              | Warp                | AI 原生终端，内置 Agent 模式和智能命令建议，仅限 terminal 使用，无 IDE 扩展      |
| [Crush](https://github.com/charmbracelet/crush)                                           | Charmbracelet       | 开源 TUI 编码 Agent，由 Charmbracelet 出品，terminal 界面，支持多种 LLM 后端     |

---

## Benchmarks

截止 2025 年 10 月，各 Agent + LLMs 在指令遵循上的测试得分如下。

### Claude Sonnet 4.5

| 模型信息 | 详情 |
| -------- | ---- |
| **提供方** | [Anthropic](https://www.anthropic.com) |
| **Pricing** | $15/1M input tokens, $75/1M output tokens |
| **开发平台** | [Anthropic Console](https://console.anthropic.com) |

| 排名 | Agent         | 得分  | 备注                                  |
| ---- | ------------- | ----- | ------------------------------------- |
| 1    | Zed AI        | 27034 | 有过度工程化（over-engineering）倾向  |
| 2    | Warp          | 27024 | 只有 terminal 终端，没有提供 IDE 扩展 |
| 3    | Claude Code   | 26930 |                                       |
| 4    | RooCode       | 26874 |                                       |
| 5    | RooCode - Med | 26750 |                                       |

### GPT-5 / Codex（Medium / High 模式）

| 模型信息 | 详情 |
| -------- | ---- |
| **提供方** | [OpenAI](https://www.openai.com) |
| **Pricing** | Codex CLI Medium: $5/1M tokens, Codex CLI High: $15/1M tokens |
| **开发平台** | [OpenAI Platform](https://platform.openai.com) |

| 排名 | Agent            | 得分  | 备注 |
| ---- | ---------------- | ----- | ---- |
| 1    | Codex CLI High   | 26734 |      |
| 2    | Codex CLI Medium | 26218 |      |
| 3    | Windsurf         | 25850 |      |
| 4    | FactoryCLI       | 25042 |      |
| 5    | opencode         | 24050 |      |

### GLM 4.6（智谱）

| 模型信息 | 详情 |
| -------- | ---- |
| **提供方** | [智谱 AI](https://zhipuai.cn) |
| **Pricing** | ¥5/1M tokens (国内), $0.7/1M tokens (国际) |
| **开发平台** | [智谱开放平台](https://open.bigmodel.cn) |

| 排名 | Agent    | 得分  | 备注                                 |
| ---- | -------- | ----- | ------------------------------------ |
| 1    | Zed AI   | 26074 | 有过度工程化（over-engineering）倾向 |
| 2    | Crush    | 25082 |                                      |
| 3    | RooCode  | 24274 |                                      |
| 4    | opencode | 23554 |                                      |

### Grok Code Fast 1

| 模型信息 | 详情 |
| -------- | ---- |
| **提供方** | [xAI](https://x.ai) |
| **Pricing** | $0.50/1M input tokens, $15/1M output tokens |
| **开发平台** | [xAI Console](https://console.x.ai) |

| 排名 | Agent          | 得分  |
| ---- | -------------- | ----- |
| 1    | RooCode        | 24270 |
| 2    | GitHub Copilot | 23814 |
| 3    | Cursor         | 23270 |
| 4    | Windsurf       | 23154 |
| 5    | Crush          | 19114 |

---

### 综合对比总表

> 各模型下各 Agent 的排名综合（- 表示未参与测试，✓ 表示进入前五）

| Agent            | Claude Sonnet 4.5 | GPT-5 Codex   | GLM 4.6 | Grok Code Fast 1 |
| ---------------- | ----------------- | ------------- | ------- | ---------------- |
| Zed AI           | 🥇 1st             | -             | 🥇 1st   | -                |
| Codex CLI High   | -                 | 🥇 1st (26734) | -       | -                |
| Codex CLI Medium | -                 | 🥈 2nd (26218) | -       | -                |
| Windsurf         | -                 | 🥉 3rd (25850) | -       | 4th (23154)      |
| FactoryCLI       | -                 | 4th (25042)   | -       | -                |
| opencode         | 5th               | 5th (24050)   | 4th     | -                |
| Claude Code      | 🥉 3rd             | -             | -       | -                |
| Warp             | 🥈 2nd (27024)     | -             | -       | -                |
| RooCode          | 4th               | -             | 🥉 3rd   | 🥇 1st (24270)    |
| RooCode - Med    | 5th               | -             | -       | -                |
| Crush            | -                 | -             | 🥈 2nd   | 5th (19114)      |
| GitHub Copilot   | -                 | -             | -       | 🥈 2nd (23814)    |
| Cursor           | -                 | -             | -       | 🥉 3rd (23270)    |

备注:

- Zed AI 由于过度工程化，任务执行时常过长且 token 消耗量偏大
- 开源 Agent 工具目前挑选 RooCode 和 opencode 即可
- ~~RooCode + Grok Code Fast 1 的组合凭借其极低的使用成本，成为最具性价比的组合。~~