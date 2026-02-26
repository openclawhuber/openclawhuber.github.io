---
layout: post
title: "Setting Up Telegram Bot with OpenClaw"
date: 2025-01-20
categories: [setup, guide]
tags: [openclaw, telegram, bot, messaging]
description: "Connect OpenClaw to Telegram and get a personal AI assistant in your pocket. Full setup walkthrough."
---

Telegram is the most popular messaging channel for OpenClaw users, and for good reason: it's fast, has excellent bot support, rich media handling, and works on every platform. Here's how to set it up.

## Step 1: Create Your Bot

Open Telegram and message [@BotFather](https://t.me/botfather):

1. Send `/newbot`
2. Choose a display name (e.g., "My AI Assistant")
3. Choose a username (must end in `bot`, e.g., `myai_assistant_bot`)
4. BotFather gives you a token — save this securely

**Important:** Never share your bot token. Anyone with it can control your bot.

## Step 2: Configure OpenClaw

Add the Telegram channel to your OpenClaw config. You can do this through the CLI:

```bash
openclaw channel add telegram
```

When prompted, paste your bot token. OpenClaw stores it securely in your system keychain.

## Step 3: Set Your User ID

You want the bot to respond *only to you*, not random people who find it. Get your Telegram user ID by messaging [@userinfobot](https://t.me/userinfobot), then add it to your allowlist:

```bash
openclaw config set telegram.allowedUsers "YOUR_USER_ID"
```

This is critical security. Without an allowlist, anyone who discovers your bot's username can interact with your AI agent.

## Step 4: Test the Connection

Send any message to your bot. You should get an AI-generated response within a few seconds. If not:

- Check that the gateway is running: `openclaw gateway status`
- Verify the token: `openclaw channel test telegram`
- Check logs: `openclaw gateway logs`

## Telegram-Specific Features

OpenClaw supports several Telegram-native features:

- **Voice messages** — send voice notes and OpenClaw transcribes them
- **Photos** — send images for analysis
- **Documents** — share PDFs, spreadsheets, etc.
- **Reactions** — the bot can react to your messages with emoji
- **Silent messages** — late-night updates won't buzz your phone

## Pro Tips

1. **Pin your bot chat** — you'll use it constantly, keep it at the top
2. **Use /commands** — set up custom commands via BotFather for quick actions
3. **Enable notifications selectively** — mute the chat but allow mentions
4. **Create a group** — add your bot to a group with family or team members for shared AI access (adjust allowlist accordingly)

## Privacy Mode

By default, Telegram bots in groups only see messages that mention them or start with a command. If you want your bot to see all group messages (for context), disable privacy mode via BotFather:

Send `/setprivacy` to BotFather, select your bot, and choose "Disable."

## What Can You Do With It?

Once connected, your Telegram bot becomes a direct line to your AI agent. Ask it to:
- Check your calendar
- Draft emails
- Research topics
- Manage tasks
- Control smart home devices
- Anything your OpenClaw skills support

The beauty of Telegram is that it works everywhere — phone, tablet, desktop, web. Your AI assistant is always one message away.

---

*Struggling with Telegram setup? I can have your bot configured and secured in under an hour. [View my services](/services/) or email openclawhuber@gmail.com.*
