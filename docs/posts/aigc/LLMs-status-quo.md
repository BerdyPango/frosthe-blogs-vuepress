---
title: 大语言模型现状
author: Frost He
excerpt: 将当前 GenAI 领域的各个工具和模型分门别类进行索引，用于快速查找
date: 2025-08-20
category:
- GenAI
tag:
- ai
- llms
---

# 大语言模型现状

## 实用网站链接

- [大模型 API 聚合平台](https://www.dmxapi.cn/pricing): 统一的入口方便实时查看各个大模型服务提供商的 API 价格

## 大模型社区

- [HuggingFace🔗](https://huggingface.co/): 全球最大的大模型开源社区。提供模型、数据集和空间托管，社区化协作，原生为公有云服务。
- [HuggingFace Leaderboard🔗](https://huggingface.co/spaces/open-llm-leaderboard/open_llm_leaderboard#/?official=true): 比较市场上的各类大模型
- [HuggingFace Spaces🔗](https://huggingface.co/spaces): LLM 社区免费 Host 前端页面的 Hub 产品。
- [ModelScope🔗](https://modelscope.cn/home): 国内大模型开源社区，大部分都为国内开源大模型，可作为 HuggingFace 由于网络原因的替代
- [Ollama](https://ollama.com): 开源大模型的运行时环境工具，类似于 Docker 概念
- [XInference](https://github.com/xorbitsai/inference): 开源聚合 LLM 推理服务，可集成几乎所有 LLM 及多模态模型并暴露统一的 API，支持 Docker 部署

___
## 推理

- [Deepseek R1](https://www.deepseek.com/)
- [OpenAI/o1](https://openai.com/o1/)，闭源、国内网络不可用

## 生图

- [OpenAI ChatGPT o1](https://openai.com/o1/)
- [BAGEL](https://bagel-ai.org)

## 生视频

- [Google Veo3](https://veo3.ai/)
- [Runway](https://runwayml.com/)
- [OpenAI Sora](https://openai.com/index/sora-system-card/)
- [Wan-AI](https://huggingface.co/Wan-AI)

## 指令遵循

- [OpenAI/GPT4.5](https://openai.com/index/introducing-gpt-4-5/)，闭源、国内网络不可用

## 编码

- [Anthropic Claude Code](https://www.anthropic.com/claude-code): 闭源、国内网络不可用
- [OpenAI Codex](https://openai.com/codex/)
- [Google Gemini CLI](https://github.com/google-gemini/gemini-cli)
- [Opencode](https://github.com/sst/opencode): CLI 界面, CC, gemini, codex 的开源替代

## World Model

- [Google DeepMind Genie](https://deepmind.google/discover/blog/genie-2-a-large-scale-foundation-world-model/)

___

## 其他分类

### 开源模型

- [DeepSeek🔗](https://www.deepseek.com/)
  - [DeepSeek-R1@Ollama](https://ollama.com/library/deepseek-r1)
  - [DeepSeek@HuggingFace](https://huggingface.co/deepseek-ai/DeepSeek-R1)
- [QWQ-32B🔗](https://qwenlm.github.io/zh/blog/qwq-32b/): 阿里开源的强化学习 32B 推理模型
  - [QWQ@Ollama](https://ollama.com/library/qwq)
  - [QWQ-32B@HuggingFace](https://huggingface.co/Qwen/QwQ-32B)
  - [QWQ-32B@ModelScope](https://modelscope.cn/models/Qwen/QwQ-32B)
- [Llama](https://www.llama.com/): 由 Meta 公司开源的大模型家族
  - [Llama3.3@Ollama](https://ollama.com/library/llama3.3)
  - [Llama 大模型家族](https://huggingface.co/meta-llama)
- [Phi-4-multimodal-instruct](https://techcommunity.microsoft.com/blog/aiplatformblog/introducing-phi-4-microsoft%E2%80%99s-newest-small-language-model-specializing-in-comple/4357090): 由微软开源的 Phi-4 大模型家族
  - [phi-4 多模态大模型](https://huggingface.co/microsoft/Phi-4-multimodal-instruct)
  - [phi-4 大模型家族](https://huggingface.co/collections/microsoft/phi-4-677e9380e514feb5577a40e4)
  - [phi-4-14b@ollama](https://ollama.com/library/phi4)
- [Claude](https://www.anthropic.com/claude): Anthropic 的商用大语言模型，目前国内无法通过正常渠道访问。
  - Claude 3.5 生成的代码以简洁性、可读性和注释完整性著称，尤其在软件工程中表现突出。
  - 支持 200K tokens 上下文窗口，适合处理多步骤任务（如跨文件代码重构、复杂业务流程分解）。
  - 在工具调用（如 API 集成）和智能体任务（如自动化代码评审）中表现卓越，支持端到端软件开发流程
- [MiniMax](https://chat.minimaxi.com/)
  - [minimax-01⭐2.4k](https://github.com/MiniMax-AI/MiniMax-01): 擅长处理上下文，4M token，API 价格非常便宜
- [硅基流动](https://siliconflow.cn/zh-cn/): 提供各种开源大模型的 API 服务，包括嵌入和重排序模型

### 词嵌入模型 Embedding

- [nomic-ai🔗](https://huggingface.co/nomic-ai)
  - [nomic-embed-text@Huggingface](https://huggingface.co/nomic-ai/nomic-embed-text-v2-moe)
  - [nomic-embed-text@Ollama](https://ollama.com/library/nomic-embed-text)
- [BAAI/bge-large-zh-v1.5](): 由 BAAI(北智研究院) 推出的系列模型

### 重排序模型 Reranking

- [Cohere Reranking](https://cohere.com/rerank)
- [BAAI/bge-reranker-v2-m3](): 由 BAAI(北智研究院) 推出的系列模型