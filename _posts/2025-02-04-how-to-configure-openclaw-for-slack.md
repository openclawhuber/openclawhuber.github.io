---
layout: post
title: "How to Configure OpenClaw for Slack"
date: 2025-02-04
categories: [setup, guide]
tags: [openclaw, slack, workplace, messaging]
description: "Add an AI-powered assistant to your Slack workspace with OpenClaw. Setup guide for business teams."
---

Slack is where work happens for millions of teams. Adding an OpenClaw-powered assistant to your workspace means AI help is always a message away — no context switching, no separate apps.

## Creating a Slack App

1. Visit [api.slack.com/apps](https://api.slack.com/apps) and click "Create New App"
2. Choose "From scratch" and name your app
3. Select your workspace

## Required Scopes

Under OAuth & Permissions, add these Bot Token Scopes:

- `chat:write` — Send messages
- `channels:history` — Read channel messages
- `im:history` — Read DMs
- `reactions:write` — Add emoji reactions
- `files:read` — Access shared files

Install the app to your workspace and copy the Bot User OAuth Token.

## OpenClaw Configuration

```bash
openclaw channel add slack
```

Enter your bot token and signing secret (found under Basic Information in your Slack app settings).

## Event Subscriptions

Slack uses event-based communication. You'll need to configure:

1. Enable Event Subscriptions in your Slack app
2. Set the Request URL to your OpenClaw gateway's webhook endpoint
3. Subscribe to: `message.channels`, `message.im`, `app_mention`

If your OpenClaw instance isn't publicly accessible, use a tunnel service or configure Slack's Socket Mode instead:

```bash
openclaw config set slack.socketMode true
```

Socket Mode doesn't require a public URL — it uses WebSocket connections initiated from your end.

## Best Practices for Slack

**DM-first approach:** Let users DM the bot for personal requests. Use channels for team-wide information.

**App Home:** Configure the App Home tab in Slack to show a welcome message and common commands.

**Unfurl links:** Your bot can automatically summarize links shared in channels — useful for busy executives who don't have time to click every link.

**Scheduled messages:** Use OpenClaw's cron system to send daily summaries or reminders via Slack.

## The Business Case

For small businesses already on Slack, adding an AI assistant costs:
- $0 for the Slack integration (your existing workspace)
- API costs for the AI model ($5-20/month typical)
- OpenClaw hosting (your own hardware or a cheap VPS)

Compare that to hiring a human assistant or paying for enterprise AI tools. The ROI is immediate.

---

*I specialize in setting up AI assistants for business teams. [View pricing](/services/) or reach out at openclawhuber@gmail.com for a custom quote.*
