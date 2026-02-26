---
layout: post
title: "OpenClaw Cron Jobs: Automate Everything"
date: 2025-08-12
categories: [technical]
tags: [openclaw, cron, automation, scheduling]
description: "Use OpenClaw's cron system to schedule recurring tasks, reports, and automated workflows."
---

Cron jobs in OpenClaw let you schedule tasks that run automatically. Daily reports, weekly summaries, periodic checks — anything your agent can do on demand, it can do on a schedule.

## How OpenClaw Cron Works

OpenClaw has its own cron system that triggers AI agent sessions at scheduled times. Unlike traditional cron (which runs scripts), OpenClaw cron starts an AI conversation with a specific prompt.

## Setting Up a Cron Job

```bash
openclaw cron add --schedule "0 8 * * *" --prompt "Send me a morning briefing with today's calendar, weather, and any urgent emails" --channel telegram
```

This runs every day at 8:00 AM and delivers the result to your Telegram.

## Practical Cron Jobs

### Morning Briefing (Daily, 8 AM)
```
Schedule: 0 8 * * 1-5
Prompt: "Morning briefing: today's calendar events, urgent emails from overnight, weather forecast, and any pending tasks."
```

### End-of-Day Summary (Daily, 5 PM)
```
Schedule: 0 17 * * 1-5
Prompt: "End of day summary: what was completed today, any pending items, tomorrow's schedule preview."
```

### Weekly Report (Monday, 9 AM)
```
Schedule: 0 9 * * 1
Prompt: "Weekly summary: key events from last week, this week's calendar overview, pending follow-ups, and any overdue items."
```

### Security Check (Weekly, Sunday)
```
Schedule: 0 3 * * 0
Prompt: "Run a security check: verify gateway status, check for OpenClaw updates, review recent access logs, run openclaw-audit."
```

### Invoice Reminder (Monthly, 1st)
```
Schedule: 0 10 1 * *
Prompt: "Check for any overdue invoices and send reminders to clients who haven't paid."
```

## Cron vs Heartbeat

OpenClaw also has a heartbeat system for periodic checks. When to use which:

**Cron:** Exact timing matters, isolated sessions, different model/thinking level, one-shot tasks.

**Heartbeat:** Batch multiple checks together, needs conversation context, timing can drift, reduce API calls.

## Managing Cron Jobs

```bash
# List all cron jobs
openclaw cron list

# Remove a cron job
openclaw cron remove <id>

# Pause/resume
openclaw cron pause <id>
openclaw cron resume <id>
```

## Cost Considerations

Each cron job triggers an AI session, which costs API tokens. A morning briefing might cost $0.02-0.10 depending on the model. Running 5 cron jobs daily = $0.10-0.50/day = $3-15/month.

Choose lighter models for simple, recurring tasks to keep costs down.

---

*Want automated reports and scheduled tasks? [My setup service](/services/) includes configuring cron jobs tailored to your daily workflow.*
