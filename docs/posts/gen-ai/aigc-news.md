---
title: GenAI 新闻收集
author: Frost He
excerpt: 从各渠道收集到的 GenAI 新闻
date: 2026-03-25
category:
- GenAI
tag:
- ai
- llms
---

# GenAI News from all Channels

## 2026.03.24 - Google DeepMind 与 Agile Robots 达成战略合作

[Google partners with Agile Robots](https://www.cnbc.com/2026/03/24/google-agile-robots-ai-robotics.html)

Agile Robots 将集成 Google DeepMind 的 Gemini Robotics foundation models，双方将在电子制造、汽车、数据中心和物流等领域测试和部署机器人。此前 Boston Dynamics 也宣布了与 Google DeepMind 的类似合作。

## 2026.03.24 - Anthropic 发布 Claude Code Auto Mode / Computer Use / Channels

- [Claude Code Auto Mode](https://9to5mac.com/2026/03/24/claude-code-gives-developers-auto-mode-a-safer-alternative-to-skipping-permissions/) - 更安全的自主执行模式，通过 classifier 检测潜在破坏性操作
- [Claude Computer Use](https://www.cnbc.com/2026/03/24/anthropic-claude-ai-agent-use-computer-finish-tasks.html) - Claude 可以操作电脑完成任务（点击、滚动、导航网页和应用）
- [Claude Code Channels](https://venturebeat.com/orchestration/anthropic-just-shipped-an-openclaw-killer-called-claude-code-channels) - 通过 Discord 和 Telegram 与 Claude Code 交互

## 2026.03.17 - Google Gemini 3 Deep Think 重大升级

[Gemini 3 Deep Think](https://blog.google/innovation-and-ai/models-and-research/gemini-models/gemini-3-deep-think/)

专为科学、研究和工程场景设计的推理模式，在 Humanity's Last Exam 上达到 48.4%（无工具），在 ARC-AGI-2 上达到 84.6%。首次通过 Gemini API 向研究人员和企业开放。

## 2026.03.17 - OpenAI 发布 GPT-5.4 mini 和 nano

[Introducing GPT-5.4 mini and nano](https://openai.com/index/introducing-gpt-5-4-mini-and-nano/)

将 GPT-5.4 的核心能力带到更快、更高效的小模型中，面向高吞吐场景。

## 2026.03.05 - OpenAI 发布 GPT-5.4

[Introducing GPT-5.4](https://openai.com/index/introducing-gpt-5-4/)

提供 Thinking（推理优先）和 Pro（最强性能）两个变体，上下文窗口达 105 万 tokens。整合了 GPT-5.3-Codex 的编程能力、改进的推理能力和自主操控桌面/浏览器的 agentic 能力。比 GPT-5.2 减少 33% 的事实错误。

## 2026.03.01 - Alibaba 发布 Qwen 3.5 Small 系列

提供 0.8B / 2B / 4B / 9B 四个 dense 模型，原生多模态（文本、图像、视频），9B 模型在 GPQA Diamond 上得分 81.7，超过 GPT-OSS-120B 的 71.5。

## 2026.02 - Anthropic 发布 Claude 4.6 系列 (Opus 4.6 & Sonnet 4.6)

Opus 4.6 和 Sonnet 4.6 均支持 100 万 token 上下文窗口（beta）。Sonnet 4.6 为 claude.ai 免费默认模型，定价 $3/$15 per million tokens。Claude Code 的年化收入突破 25 亿美元。

## 2026.02 - Google Gemini 3.1 Pro / Flash-Lite

- Gemini 3.1 Pro：最先进的 Pro 级模型，100 万 token 上下文窗口，ARC-AGI-2 达 77.1%
- Gemini 3.1 Flash-Lite：比之前版本响应速度快 2.5 倍，输出快 45%，仅 $0.25/M input tokens

## 2026.02.17 - xAI 发布 Grok 4.20

引入四 agent 并行处理架构 - Grok 负责整体协调、Harper 负责事实核查、Benjamin 负责逻辑与编程、Lucas 负责创意推理。

## 2026.02 - OpenClaw 被 OpenAI 收购

从个人开源项目到 68,000 GitHub stars，超越 React 和 Linux 成为 GitHub 最高 star 项目，后被 OpenAI 收购。

## 2026.01.29 - Google Project Genie 面向公众开放

[Project Genie](https://blog.google/innovation-and-ai/models-and-research/google-deepmind/project-genie/)

基于 Genie 3 世界模型的实验性产品，用户可通过文本和图像 prompt 生成并探索实时交互式虚拟环境。目前面向美国 Google AI Ultra 订阅用户开放。

## 2025.12 - Meta 收购 Manus

Meta 以约 20 亿美元收购了 agentic AI 平台 [Manus](https://manus.im/)。

## 2025.11 - Google DeepMind 发布 SIMA 2

- [SIMA 2 Blog](https://deepmind.google/blog/sima-2-an-agent-that-plays-reasons-and-learns-with-you-in-virtual-3d-worlds/)
- [Technical Report (arXiv)](https://arxiv.org/html/2512.04797v1)

SIMA（Scalable Instructable Multiworld Agent）的第二代，集成 Gemini 2.5 Flash Lite 作为推理引擎，从单纯的指令执行者进化为交互式游戏伙伴。主要进步：

- 多模态输入：支持语音指令、屏幕涂鸦、emoji 等多种交互方式
- 自我改进：利用 Gemini 自主生成任务和奖励，在新环境中从零学习技能
- 泛化能力：可泛化到 Genie 3 生成的逼真环境中

## 2025.08.27 - Google 发布 NanoBanana / Gemini 2.5 Flash Image Preview

发布并集成至 [Google AI Studio](https://aistudio.google.com/apps)

## 2025.08.21 - DeepSeek-V3.1 发布

[DeepSeek-V3.1](https://api-docs.deepseek.com/zh-cn/news/news250821) 主要变化：

- 混合推理架构：一个模型同时支持思考模式与非思考模式；
- 更高的思考效率：相比 DeepSeek-R1-0528，DeepSeek-V3.1-Think 能在更短时间内给出答案；
- 更强的 Agent 能力：通过 Post-Training 优化，新模型在工具使用与智能体任务中的表现有较大提升

DeepSeek API 也已同步升级，`deepseek-chat` 对应非思考模式，`deepseek-reasoner` 对应思考模式，且上下文均已扩展为 128K。同时，API Beta 接口支持了 strict 模式的 `Function Calling`，以确保输出的 Function 满足 schema 定义。详见 [Function Calling](https://api-docs.deepseek.com/zh-cn/guides/function_calling)

## 2025.08.21 - Context as Memory

偏向内存一致性的视频生成策略 [Context as Memory@GitHub](https://context-as-memory.github.io/)

## 2025.08.08 - OpenAI 宣布 GPT5

OpenAI [GPT-5 is here](https://openai.com/gpt-5/)

## 2025.08.06 - Google 发布 Genie 3 世界模型

[Genie 3: A new frontier for world models](https://deepmind.google/discover/blog/genie-3-a-new-frontier-for-world-models/)

General purpose world model that can generate an unprecedented diversity of interactive environments.

## 2025.08.06 - OpenAI 发布开源模型

[Open models by OpenAI](https://openai.com/open-models/)

20B and a 120B, both are MoE models, with very good quality. License is Apache 2.0.

## 2025.08.05 - Google NotebookLM 引入视频模式(video mode)

[Google NotebookLM](https://notebooklm.google/)

## 2025.07.19 - Universal Tool Calling Protocal(UTCP) - 更快、伸缩性更强的 MCP 替代协议

- [Reddit thread](https://www.reddit.com/r/LocalLLaMA/comments/1lzl5zk/utcp_a_safer_scalable_toolcalling_alternative_to/)
- [universal-tool-calling-protocol@GitHub](https://github.com/universal-tool-calling-protocol)

## 2025.07.09 - LM Studio Allows Use at Work

- [X(Tweets)](https://x.com/lmstudio/status/1942628356924596567?s=19)
- [LM Studio](https://lmstudio.ai/)

## 2025.06.30 - Google Gemma 3n: a new open-weight language model designed to run locally on your hardware

- [Introducing Gemma 3n: The developer guide](https://developers.googleblog.com/en/introducing-gemma-3n-developer-guide/)
- [gemma3n@huggingface](https://huggingface.co/blog/gemma3n)

## 2025.06.30 - Opencode: CLI based, alternative to CC, gemini, codex

[opencode@GitHub](https://github.com/sst/opencode)

## 2025.06.25 - Gemini CLI : a free LLM based CLI that knows how to code

Looks like Claude Code, but from Google.

- [gemini-cli@GitHub](https://github.com/google-gemini/gemini-cli)
- [Blog](https://blog.google/technology/developers/introducing-gemini-cli-open-source-ai-agent/)

## 2025.06.24 - Perfect introduction to chatbots

[Using AI Right Now: A Quick Guide](https://www.oneusefulthing.org/p/using-ai-right-now-a-quick-guide)

## 2025.06.23 - Another AI-based game engine by Tencent Hunyuan

[Hunyuan-GameCraft](https://hunyuan-gamecraft.github.io/)

It creates worlds at runtime

## 2025.06.14 - Runway introduces Chat to Video

- [Introducing Chat Mode@X](https://x.com/runwayml/status/1933213502728237342)
- [Runway](https://runwayml.com/)

## 2025.05.25 - BAGEL: 开源统一多模态模型

[BAGEL](https://bagel-ai.org/)

## 2025.05.20 - DeepWiki - 生成交互式文档且支持从任意 GitHub 仓库集成 RAG

[DeepWiki](https://deepwiki.com/)

## 2025.05.18 - OpenAI 发布 Codex

[OpenAI Codex](https://openai.com/codex/)