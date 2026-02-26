---
layout: post
title: "Why Your AI Agent Needs an Allowlist"
date: 2025-04-05
categories: [security]
tags: [openclaw, allowlist, security]
description: "An allowlist is the most important security measure for your AI agent. Here's why and how to set one up."
---

I audit OpenClaw installations regularly, and the number one security issue I find is the same every time: **no allowlist configured.**

This means the AI agent responds to anyone who messages it. On Telegram, that means any of Telegram's 900 million users. On Discord, anyone in your server. This is not a theoretical risk — I've seen bots racking up hundreds of dollars in API costs from random users within days.

## What Is an Allowlist?

An allowlist is a list of user IDs that your AI agent is permitted to interact with. Messages from anyone not on the list are silently ignored.

```yaml
# OpenClaw config
security:
  allowedUsers:
    - "323936588"    # Your Telegram ID
    - "987654321"    # A trusted family member
```

## Why Not a Blocklist?

A blocklist (blocking specific bad actors) doesn't work because:
- You can't predict who will find your bot
- New accounts are free and unlimited
- You're playing whack-a-mole instead of solving the problem

An allowlist flips the model: deny by default, allow by exception. This is the foundation of all good security.

## The Risks of No Allowlist

1. **Cost explosion** — Every message costs API tokens. Unauthorized users burn your money.
2. **Data exposure** — Your agent might reveal information from your emails, calendar, or files.
3. **Abuse** — Someone could use your agent for their own purposes, potentially generating harmful content under your API account.
4. **Prompt injection** — Malicious users could try to manipulate your agent into performing unauthorized actions.

## Setting It Up

For each channel in OpenClaw:

```bash
# Telegram
openclaw config set telegram.allowedUsers "YOUR_ID"

# Discord  
openclaw config set discord.allowedUsers "YOUR_DISCORD_ID"

# Global fallback
openclaw config set security.defaultPolicy "deny"
```

The global `defaultPolicy: deny` ensures that any new channel you add starts locked down.

## Testing Your Allowlist

After configuring, test from a different account. Send a message to your bot — it should be silently dropped. No response, no error, no acknowledgment.

Check the logs to confirm:
```bash
openclaw gateway logs | grep "denied"
```

You should see entries like: `Message from unauthorized user 12345678 denied`

## Grace Period for New Users

If you want to allow specific people access temporarily (say, for a demo), use time-limited tokens instead of adding them to the permanent allowlist.

An allowlist takes 30 seconds to configure and prevents 90% of security issues. There is no reason not to have one. Do it now.

---

*Run `npx openclaw-audit` to check if your allowlist is properly configured. For a comprehensive security review, check out my [$75 audit service](/services/).*
