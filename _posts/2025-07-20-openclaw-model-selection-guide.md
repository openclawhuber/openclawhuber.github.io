---
layout: post
title: "OpenClaw Model Selection Guide: Which AI to Use When"
date: 2025-07-20
categories: [technical]
tags: [openclaw, models, ai, configuration]
description: "Not all AI models are created equal. Here's when to use Claude, GPT-4, Gemini, and lighter models in OpenClaw."
---

OpenClaw supports multiple AI providers and models. Choosing the right one for each task can save you money and improve performance. Here's my field guide.

## The Models

**Claude (Anthropic)**
- Opus: Most capable, best for complex reasoning. Expensive (~$15/M input tokens).
- Sonnet: Great balance of capability and cost. My default recommendation.
- Haiku: Fast and cheap. Perfect for simple tasks.

**GPT-4o (OpenAI)**
- Strong all-rounder. Good at coding and structured output.
- GPT-4o-mini: Budget option, surprisingly capable for simple tasks.

**Gemini (Google)**
- Large context window (1M+ tokens). Great for processing long documents.
- Flash: Fast and affordable for quick tasks.

## My Recommended Setup

```yaml
models:
  default: claude-sonnet  # Daily driver
  complex: claude-opus    # Deep analysis, important decisions
  simple: claude-haiku    # Triage, categorization, simple responses
  coding: gpt-4o          # Code generation and review
  documents: gemini-pro   # Long document processing
```

## When to Use What

**Use Opus/GPT-4o for:**
- Important client communications
- Complex business decisions
- Nuanced writing that needs to be perfect
- Multi-step reasoning tasks

**Use Sonnet for:**
- Daily email triage
- General conversation
- Research synthesis
- Calendar management

**Use Haiku/Mini for:**
- Simple categorization
- Yes/no decisions
- Acknowledgment messages
- Quick lookups

## Cost Impact

The difference is significant:

| Model | Cost per 1K messages (approx) |
|-------|------------------------------|
| Claude Opus | $3-5 |
| Claude Sonnet | $0.50-1.50 |
| Claude Haiku | $0.05-0.15 |
| GPT-4o | $1-3 |
| GPT-4o-mini | $0.05-0.10 |

A well-configured model routing setup can cut your API costs by 60-70% compared to using a top-tier model for everything.

## Setting Up Model Routing

OpenClaw can automatically select models based on the task:

```yaml
routing:
  email-triage: claude-haiku      # Fast categorization
  email-draft: claude-sonnet      # Quality writing
  calendar: claude-haiku          # Simple lookups
  research: claude-sonnet         # Thorough analysis
  code-review: gpt-4o            # Strong at code
```

This happens transparently. You don't need to think about which model to use — OpenClaw picks the right one based on what it's doing.

## Fallback Strategy

Always configure fallbacks in case a provider has an outage:

```yaml
fallback:
  - claude-sonnet
  - gpt-4o
  - gemini-pro
```

If Claude is down, OpenClaw automatically switches to GPT-4o. Your assistant stays online no matter what.

---

*Not sure which models to use? [My setup service](/services/) includes model selection and routing configuration optimized for your specific use case and budget.*
