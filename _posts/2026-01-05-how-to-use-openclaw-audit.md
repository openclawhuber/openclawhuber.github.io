---
layout: post
title: "How to Use openclaw-audit to Secure Your Setup"
date: 2026-01-05
categories: [tutorial, security]
tags: [openclaw-audit, security, tutorial, tool]
description: "A complete walkthrough of the openclaw-audit tool: what it checks, how to interpret results, and how to fix issues."
---

`openclaw-audit` is a free, open-source tool that checks your OpenClaw installation for security issues and misconfigurations. Here's how to use it effectively.

## Running the Audit

No installation needed — just run:

```bash
npx openclaw-audit
```

This downloads and runs the latest version. It takes 30-60 seconds to complete.

## What It Checks

The audit tool runs checks in several categories:

### Configuration Security
- ✅ Allowlist configured for each channel
- ✅ API keys stored in keychain (not plaintext)
- ✅ Debug mode disabled in production
- ✅ Default policy set to "deny"
- ✅ Rate limiting configured

### Network Security
- ✅ Gateway bound to localhost
- ✅ Firewall enabled
- ✅ Unnecessary ports closed
- ✅ SSH hardened (no password auth, no root login)
- ✅ HTTPS configured for any exposed endpoints

### System Security
- ✅ Running as non-root user
- ✅ File permissions appropriate
- ✅ Software up to date
- ✅ Disk encryption enabled
- ✅ Backup configuration present

### Operational Security
- ✅ Logging enabled
- ✅ Log rotation configured
- ✅ No sensitive data in logs
- ✅ Memory files reviewed recently

## Interpreting Results

The output uses a traffic-light system:

- 🔴 **CRITICAL** — Fix immediately. Your setup is actively vulnerable.
- 🟡 **WARNING** — Should fix soon. Not immediately dangerous but risky.
- 🟢 **PASS** — This check passed. Good job.
- ⚪ **INFO** — Informational, no action needed.

## Fixing Common Issues

### "No allowlist configured" (Critical)
```bash
openclaw config set telegram.allowedUsers "YOUR_ID"
openclaw config set security.defaultPolicy "deny"
```

### "API key in plaintext config" (Critical)
```bash
# Move to keychain (macOS)
security add-generic-password -s "openclaw-anthropic" -a "apikey" -w "sk-ant-..." -U
# Remove from config file
openclaw config unset anthropic.apiKey
```

### "Running as root" (Critical)
Create a dedicated user and transfer the installation.

### "Firewall not enabled" (Warning)
```bash
sudo ufw default deny incoming
sudo ufw allow ssh
sudo ufw enable
```

### "Software outdated" (Warning)
```bash
npm update -g openclaw
sudo apt update && sudo apt upgrade -y  # Linux
```

## Scheduling Regular Audits

Run the audit automatically:

```bash
openclaw cron add --schedule "0 3 * * 0" \
  --prompt "Run npx openclaw-audit and send me the results. Highlight any new issues." \
  --channel telegram
```

## The Goal

A perfect audit score means your setup follows all security best practices. Most installations score 70-80% on first audit and reach 95%+ after fixing the flagged issues.

Don't aim for perfection on day one. Fix the criticals immediately, warnings within a week, and revisit informational items monthly.

---

*Want help interpreting your audit results or fixing issues? [My $75 audit service](/services/) includes a personal review of your results plus hands-on fixes for critical issues.*
