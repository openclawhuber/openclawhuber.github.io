---
layout: post
title: "Setting Up Fallback Models in OpenClaw"
date: 2025-09-12
categories: [technical]
tags: [openclaw, models, fallback, reliability]
description: "Keep your AI agent running even when providers have outages. Configure automatic model fallbacks."
---

AI provider outages happen. OpenAI goes down, Anthropic has rate limits, Google's API hiccups. If your AI agent depends on a single provider, an outage means silence. Fallback models fix this.

## The Problem

You're relying on your AI agent for email triage, meeting reminders, and client communication. At 9 AM on a busy Monday, your AI provider has an outage. Your agent goes silent. You don't get your morning briefing. Emails pile up. A client's message goes unanswered.

This is avoidable.

## Configuring Fallbacks

In your OpenClaw config:

```yaml
models:
  default: anthropic/claude-sonnet
  fallback:
    - openai/gpt-4o
    - google/gemini-pro
    - anthropic/claude-haiku
```

When Claude Sonnet fails (timeout, rate limit, or error), OpenClaw automatically tries GPT-4o. If that also fails, Gemini. Then Haiku as a last resort.

## How Fallback Selection Works

OpenClaw tries the default model first. On failure, it:

1. Logs the failure
2. Tries the next model in the fallback chain
3. Adjusts the prompt if needed (different models have different formats)
4. Delivers the response normally

The end user (you, on Telegram) never sees the switch. You just get your response, maybe a second slower than usual.

## Multi-Provider Strategy

Don't put all your eggs in one basket:

| Provider | Primary Use | Fallback For |
|----------|-------------|-------------|
| Anthropic | Default | — |
| OpenAI | Coding tasks | Anthropic |
| Google | Long documents | OpenAI |

Keep API keys configured for at least two providers. The marginal cost of a second API key ($0 — you only pay when you use it) is nothing compared to the reliability gain.

## Cost Considerations

Fallback models might cost more or less than your primary. If Claude Sonnet fails and you fall back to GPT-4o, costs per request might increase slightly. If you fall back to Haiku, they decrease.

For most users, the cost difference during rare outages is negligible. Reliability trumps a few cents.

## Testing Your Fallbacks

Don't wait for a real outage. Test deliberately:

```bash
# Temporarily block your primary provider
openclaw config set models.default "invalid-model"

# Send a test message and verify fallback works
# Then restore
openclaw config set models.default "anthropic/claude-sonnet"
```

## Monitoring

Set up alerts for fallback activation:

```yaml
alerts:
  modelFallback: true
  channel: telegram
```

When a fallback triggers, you get a notification: "Primary model (claude-sonnet) unavailable. Using fallback (gpt-4o)." This way you know something's up without your agent going dark.

---

*Reliability is part of what I configure in every [setup](/services/). Your AI agent should be as reliable as your phone — always there when you need it.*
