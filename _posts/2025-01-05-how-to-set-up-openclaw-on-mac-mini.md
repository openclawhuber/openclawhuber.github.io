---
layout: post
title: "How to Set Up OpenClaw on a Mac Mini in 30 Minutes"
date: 2025-01-05
categories: [setup, guide]
tags: [openclaw, mac-mini, installation]
description: "A step-by-step guide to installing and configuring OpenClaw on an Apple Mac Mini for personal AI agent use."
---

If you've been thinking about running your own AI assistant but don't want to deal with cloud subscriptions or privacy concerns, a Mac Mini running OpenClaw is the sweet spot. Here's how to go from unboxing to a working AI agent in about 30 minutes.

## Why Mac Mini?

The Mac Mini — especially the M-series models — offers excellent performance per watt. It runs silent, sips power, and Apple Silicon handles AI workloads surprisingly well. For a personal AI agent that runs 24/7, it's hard to beat.

## Prerequisites

- A Mac Mini (M1 or newer recommended)
- macOS 14+ (Sonoma or later)
- Node.js v20+ installed
- A Telegram, Discord, or Slack account for messaging

## Step 1: Install Node.js

If you don't have Node.js yet:

```bash
# Install Homebrew if needed
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# Install Node.js
brew install node
```

Verify with `node --version` — you need v20 or higher.

## Step 2: Install OpenClaw

```bash
npm install -g openclaw
```

That's it. One command. OpenClaw installs globally and sets up its workspace at `~/.openclaw/`.

## Step 3: Configure Your Gateway

The gateway is OpenClaw's daemon — it runs in the background and handles all communication between your AI agent and the outside world.

```bash
openclaw gateway start
```

On first run, it'll walk you through basic setup: choosing a default model, setting up API keys, and configuring your first channel.

## Step 4: Connect a Messaging Channel

For Telegram (the most popular choice):

```bash
openclaw channel add telegram
```

You'll need a bot token from [@BotFather](https://t.me/botfather). The setup wizard handles the rest.

## Step 5: Test It

Send a message to your bot on Telegram. If you get a response, you're live.

## Step 6: Harden Security

Don't skip this. Run the audit tool:

```bash
npx openclaw-audit
```

This checks for common misconfigurations: open ports, weak permissions, missing allowlists. Fix everything it flags.

## What's Next?

Once you're running, explore:
- **Email triage** — let your agent handle inbox sorting
- **Calendar integration** — never miss a meeting
- **Custom skills** — build automations specific to your workflow

The Mac Mini will happily run OpenClaw 24/7 using under 10 watts. Set it and forget it.

---

*Need help with setup? I offer [complete setup packages for $250](/services/) — from bare hardware to working agent in a day. Or run `npx openclaw-audit` to check your existing installation.*
