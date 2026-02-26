---
layout: post
title: "How to Back Up Your OpenClaw Configuration"
date: 2026-02-15
categories: [tutorial]
tags: [openclaw, backup, maintenance, operations]
description: "Don't lose your AI agent's configuration, memory, and personality. Here's how to back everything up properly."
---

Your OpenClaw agent has accumulated weeks or months of configuration, memory, and personalization. Losing it to a disk failure or accidental deletion would be painful. Back it up.

## What to Back Up

### Critical (Back Up Daily)

```
~/.openclaw/workspace/
├── SOUL.md          # Agent personality
├── AGENTS.md        # Operational rules
├── MEMORY.md        # Long-term memory
├── USER.md          # User profile
├── TOOLS.md         # Tool configurations
├── memory/          # Daily notes
│   └── *.md
└── HEARTBEAT.md     # Heartbeat config
```

### Important (Back Up Weekly)

```
~/.openclaw/
├── config.yaml      # Main configuration
├── skills/          # Custom skills
└── cron/            # Scheduled jobs
```

### Nice to Have (Back Up Monthly)

- Conversation logs
- Analytics data
- Custom scripts

## Backup Methods

### Method 1: Git (Recommended)

Your workspace is already a natural git repository:

```bash
cd ~/.openclaw/workspace
git init
git add -A
git commit -m "Backup $(date +%Y-%m-%d)"
git remote add backup git@github.com:yourusername/openclaw-backup.git
git push backup main
```

Make this a cron job for daily backups:

```bash
# Add to crontab
0 2 * * * cd ~/.openclaw/workspace && git add -A && git commit -m "Auto-backup $(date +\%Y-\%m-\%d)" && git push backup main
```

**Important:** Make the backup repo private. Your configuration contains personal information.

### Method 2: Rsync to External Storage

```bash
rsync -avz ~/.openclaw/ /Volumes/ExternalDrive/openclaw-backup/
```

Or to a remote server:

```bash
rsync -avz ~/.openclaw/ user@backupserver:~/openclaw-backup/
```

### Method 3: Cloud Storage (Encrypted)

Use `rclone` to sync to encrypted cloud storage:

```bash
rclone sync ~/.openclaw/ encrypted-remote:openclaw-backup/
```

Configure rclone with encryption to protect sensitive data.

## Restore Procedure

If you need to restore:

```bash
# From git
git clone git@github.com:yourusername/openclaw-backup.git ~/.openclaw/workspace

# From rsync
rsync -avz /Volumes/ExternalDrive/openclaw-backup/ ~/.openclaw/

# Then restart
openclaw gateway restart
```

## Testing Your Backups

A backup you haven't tested is a backup that might not work. Quarterly:

1. Restore to a temporary location
2. Verify all files are present
3. Check file integrity (no corruption)
4. Optionally: spin up a test instance with the restored config

## Automation with OpenClaw

Use your agent to manage its own backups:

```
Every night at 2am:
1. Commit any changes to the backup git repo
2. Push to remote
3. Verify the push succeeded
4. Log the backup status in memory/
```

Your agent backs itself up. Meta, but practical.

## Don't Forget

- **API keys** are in the keychain, not in files. Back up keychain separately (macOS: export from Keychain Access).
- **System-level config** (systemd services, firewall rules) should be documented somewhere.
- **Test restores** periodically. The worst time to discover your backup doesn't work is when you need it.

---

*Backup configuration is included in my [maintenance service](/services/). $50/month gets you automated backups, monitoring, and peace of mind.*
