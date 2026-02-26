---
layout: post
title: "OpenClaw Discord Integration Step by Step"
date: 2025-01-28
categories: [setup, guide]
tags: [openclaw, discord, bot, messaging]
description: "Set up OpenClaw as a Discord bot for your server. Complete integration guide with permissions and security."
---

Discord isn't just for gamers anymore. If your team communicates on Discord, adding an OpenClaw-powered AI assistant can be a game-changer. Here's the full setup.

## Creating a Discord Application

1. Go to the [Discord Developer Portal](https://discord.com/developers/applications)
2. Click "New Application" and name it
3. Go to the "Bot" section and click "Add Bot"
4. Copy the bot token — store it securely
5. Under "Privileged Gateway Intents," enable **Message Content Intent**

## Setting Permissions

Your bot needs specific permissions. Under OAuth2 → URL Generator:

- **Scopes:** `bot`, `applications.commands`
- **Bot Permissions:** Send Messages, Read Message History, Add Reactions, Embed Links, Attach Files

Generate the invite URL and add the bot to your server.

## Configuring OpenClaw

```bash
openclaw channel add discord
```

Paste your bot token when prompted. Then configure which channels the bot should monitor:

```bash
openclaw config set discord.channels "general,ai-assistant"
```

## Channel Strategy

Don't let the bot respond in every channel. Best practice:

- **Dedicated channel** — Create an `#ai-assistant` channel where the bot always responds
- **Mention-only elsewhere** — In other channels, the bot only responds when @mentioned
- **Thread support** — The bot can participate in threads for focused conversations

## Group Chat Behavior

Unlike Telegram's 1:1 model, Discord is inherently multi-user. OpenClaw handles this well:

- It tracks conversation context per channel
- It can distinguish between users
- It knows when to stay quiet (not every message needs a response)
- It supports reactions for lightweight acknowledgments

## Security Notes

Discord bots have broader access than Telegram bots. Harden accordingly:

1. **Restrict channels** — only enable the bot in channels it needs
2. **Set role permissions** — create a bot role with minimal permissions
3. **Log interactions** — OpenClaw logs all messages by default
4. **Review regularly** — check who's interacting with your bot

## Advanced: Slash Commands

You can register custom slash commands for common actions:

```
/calendar — Show today's schedule
/email — Check for urgent emails
/weather — Get the forecast
/task — Add a task to the list
```

These provide a clean interface for non-technical team members.

## When Discord Makes Sense

Choose Discord over Telegram when:
- Your team is already on Discord
- You want multi-user access with role-based permissions
- You need channel-based organization
- You want rich embeds and formatting

Choose Telegram when:
- It's just you (personal assistant)
- You want the simplest possible setup
- Mobile experience is priority

---

*Need help integrating OpenClaw with Discord? [My setup service](/services/) covers any messaging platform. $250 for a complete, secured installation.*
