# OCI Generative AI — Model Comparison

> **Live site → [enricopesce.github.io/oci-llm-comparison](https://enricopesce.github.io/oci-llm-comparison/)**

A single-page reference comparing all models available on **Oracle Cloud Infrastructure (OCI) Generative AI**, updated February 2026.

## What's inside

| Section | Details |
|---------|---------|
| **Chat models** | Cohere Command A family · Google Gemini 2.5 · Meta Llama 4 · OpenAI gpt-oss · xAI Grok 4 |
| **Embedding models** | Cohere Embed v3 & v4 (text + multimodal) |
| **Rerank model** | Cohere Rerank 3.5 |
| **Selection guide** | Use-case decision matrix (RAG, agentic, coding, multimodal, fine-tuning) |

**Columns covered:** Model ID · Tier · Context window · Multimodal · Tool use · Fine-tuning · Reasoning · Status · Best for

## Features

- 30+ models across 5 providers
- Dark / Light mode toggle (preference saved in `localStorage`)
- Fully static — no JavaScript framework, no build step
- Mobile responsive

## Data source

All data sourced from the [OCI official documentation](https://docs.oracle.com/en-us/iaas/Content/generative-ai/).

## Providers

| Provider | Models |
|----------|--------|
| [Cohere](https://cohere.com) | Command A Reasoning, Command A Vision, Command A, Command R+, Command R, Embed v4/v3, Rerank 3.5 |
| [Google](https://deepmind.google/gemini) | Gemini 2.5 Pro, Flash, Flash-Lite |
| [Meta](https://ai.meta.com/llama/) | Llama 4 Maverick, Llama 4 Scout, Llama 3.3 70B, Llama 3.2 90B/11B Vision, Llama 3.1 405B |
| [OpenAI](https://openai.com) | gpt-oss-120b, gpt-oss-20b |
| [xAI](https://x.ai) | Grok 4, Grok 4 Fast, Grok 4.1 Fast, Grok 3, Grok 3 Fast, Grok 3 Mini, Grok Code Fast 1 |
