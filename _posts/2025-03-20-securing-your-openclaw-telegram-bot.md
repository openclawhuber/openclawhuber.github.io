---
layout: post
title: "Securing Your OpenClaw Telegram Bot"
date: 2025-03-20
categories: [security]
tags: [openclaw, telegram, security, hardening]
description: "Your Telegram bot is a direct line to your AI agent. Here's how to lock it down properly."
---

Your Telegram bot has access to your email, calendar, files, and potentially your entire digital life. If someone else gains access to it, they gain access to everything your AI agent can do. Security isn't optional here.

## The Threat Model

Who might try to access your bot?

1. **Random users** who discover your bot's username
2. **Targeted attackers** who know you use an AI agent
3. **Automated scanners** that probe Telegram bots

The good news: Telegram's architecture makes most attacks impractical *if* you configure things properly.

## Step 1: Allowlist Your User ID

This is the single most important security measure. Configure OpenClaw to respond **only** to your Telegram user ID:

```bash
openclaw config set telegram.allowedUsers "YOUR_NUMERIC_ID"
```

Get your ID from [@userinfobot](https://t.me/userinfobot). Without this, your bot responds to literally anyone who messages it.

## Step 2: Disable Group Adding

Prevent others from adding your bot to groups:

In BotFather: `/setjoingroups` → Select your bot → Disable

If you do use groups, maintain a strict allowlist of group IDs.

## Step 3: Hide Your Bot's Username

Don't share your bot's @username publicly. Treat it like a password. If someone knows the username, they can try to interact with it (though the allowlist blocks them).

## Step 4: Rate Limiting

Even with allowlisting, implement rate limiting to prevent accidental or malicious flooding:

```yaml
telegram:
  rateLimit:
    messagesPerMinute: 20
    tokensPerMinute: 10000
```

## Step 5: Audit Regularly

Run `npx openclaw-audit` monthly. It checks for:

- Missing allowlists
- Overly permissive configurations
- Exposed tokens
- Unnecessary permissions

## Step 6: Token Rotation

If you suspect your bot token is compromised:

1. Go to BotFather
2. Send `/revoke` and select your bot
3. You'll get a new token
4. Update it in OpenClaw: `openclaw config set telegram.token "NEW_TOKEN"`

## Step 7: Monitor Logs

Regularly check who's attempting to interact with your bot:

```bash
openclaw gateway logs | grep telegram
```

Look for messages from unknown user IDs — these are people who found your bot.

## Common Mistakes

- ❌ No allowlist configured (bot responds to everyone)
- ❌ Bot token in a git repository
- ❌ Sharing bot username on social media
- ❌ Running with debug mode in production (logs sensitive data)
- ❌ Not updating OpenClaw (missing security patches)

Security isn't a one-time thing. It's a practice. Schedule a monthly audit, keep things updated, and treat your bot like what it is: a gateway to your digital life.

---

*Worried about your setup's security? I offer [$75 security audits](/services/) — I'll review your entire configuration and give you a prioritized list of fixes.*
