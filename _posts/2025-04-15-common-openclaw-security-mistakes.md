---
layout: post
title: "Common OpenClaw Security Mistakes (And How to Fix Them)"
date: 2025-04-15
categories: [security]
tags: [openclaw, security, mistakes, hardening]
description: "The most frequent security issues I find when auditing OpenClaw installations, with fixes for each."
---

After auditing dozens of OpenClaw installations, I've seen the same mistakes over and over. Here are the top 10, ranked by severity.

## 1. No Allowlist (Critical)

Already covered this in depth, but it bears repeating. Over 60% of installations I audit have no allowlist. Fix: `openclaw config set security.defaultPolicy "deny"` and add your user IDs.

## 2. API Keys in Git Repos (Critical)

People commit their `.openclaw` config directory to GitHub, complete with API keys. Once it's in git history, it's there forever (even if you delete the file later).

**Fix:** Add `.openclaw/` to `.gitignore`. If keys were already committed, rotate them immediately.

## 3. Running as Root (High)

Running OpenClaw as root means a compromised agent has full system access.

**Fix:** Create a dedicated user with minimal permissions.

## 4. Debug Mode in Production (High)

Debug logging often includes full message content, API keys in request headers, and other sensitive data.

**Fix:** Set `gateway.logging.level: info` (not `debug`) in production.

## 5. No Firewall (Medium)

Especially common on VPS setups. The server has all ports open to the internet.

**Fix:** `ufw default deny incoming && ufw allow ssh && ufw enable`

## 6. Outdated Software (Medium)

Old versions of Node.js, OpenClaw, or OS packages may have known vulnerabilities.

**Fix:** Set up automatic security updates. Run `npm update -g openclaw` regularly.

## 7. Weak SSH Config (Medium)

Password authentication enabled, root login allowed, default port.

**Fix:**
```bash
# /etc/ssh/sshd_config
PermitRootLogin no
PasswordAuthentication no
Port 2222  # Change from default
```

## 8. No Log Monitoring (Low)

If you're not watching the logs, you won't notice unauthorized access attempts.

**Fix:** Set up basic log monitoring. Even a daily cron job that emails you suspicious entries is better than nothing.

## 9. Overly Broad Tool Permissions (Low)

Giving your AI agent access to tools it doesn't need increases the attack surface.

**Fix:** Review enabled skills and disable anything you don't use: `openclaw skill disable <name>`

## 10. No Backup Strategy (Low)

Not a security vulnerability per se, but losing your configuration, memory, and conversation history to a disk failure is painful.

**Fix:** Automated daily backups to an external location. Even a simple `rsync` cron job works.

## The Quick Fix

Run the audit tool right now:

```bash
npx openclaw-audit
```

It catches most of these automatically and gives you step-by-step fixes.

---

*Don't want to fix these yourself? My [$75 security audit](/services/) covers all of this and more. I'll review your setup and deliver a prioritized fix list within 48 hours.*
