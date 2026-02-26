---
layout: post
title: "How to Monitor Your OpenClaw Agent"
date: 2025-09-28
categories: [technical]
tags: [openclaw, monitoring, operations, maintenance]
description: "Keep your AI agent healthy with proper monitoring: logs, alerts, performance tracking, and cost management."
---

An AI agent running 24/7 needs monitoring. Not obsessive monitoring — just enough to catch problems before they become outages.

## The Basics

### Check Gateway Status

```bash
openclaw gateway status
```

This shows: uptime, connected channels, last message time, and any errors.

### View Logs

```bash
# Recent logs
openclaw gateway logs

# Filter by level
openclaw gateway logs --level error

# Last 24 hours
openclaw gateway logs --since "24 hours ago"
```

### Check API Usage

Monitor your AI provider dashboards:
- [Anthropic Console](https://console.anthropic.com/) — Usage and costs
- [OpenAI Dashboard](https://platform.openai.com/usage) — Token usage
- [Google AI Studio](https://aistudio.google.com/) — Gemini usage

## Automated Monitoring

Set up a weekly health check cron:

```bash
openclaw cron add --schedule "0 3 * * 0" \
  --prompt "Run a health check: gateway status, recent errors, API cost for the week, disk usage, and any security issues. Send me a summary." \
  --channel telegram
```

Every Sunday at 3 AM, you get a health report. No action needed unless something's flagged.

## What to Watch For

**Error spikes:** A sudden increase in errors usually means an API issue or misconfiguration.

**Cost anomalies:** If your monthly API bill doubles, investigate. Could be a runaway process or unauthorized access.

**Response time:** If your agent takes 30+ seconds to respond, something's wrong (model overloaded, network issues, resource constraints).

**Disk usage:** Memory files and logs grow over time. Make sure you're not running out of disk.

**Channel connectivity:** Verify all messaging channels are connected and responsive.

## Setting Up Alerts

Configure OpenClaw to notify you of critical issues:

```yaml
monitoring:
  alerts:
    errorRate:
      threshold: 5  # per hour
      channel: telegram
    responseTime:
      threshold: 30s
      channel: telegram
    diskUsage:
      threshold: 90%
      channel: telegram
```

## The Lazy Monitoring Approach

If you don't want to set up anything fancy:

1. Use your agent daily (you'll notice if it breaks)
2. Run `npx openclaw-audit` monthly
3. Check your API provider bills monthly
4. Keep the software updated

That's honestly enough for most personal setups.

---

*Monitoring is included in my [$50/month maintenance service](/services/). I'll keep an eye on your agent so you don't have to.*
