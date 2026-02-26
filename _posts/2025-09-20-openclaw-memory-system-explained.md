---
layout: post
title: "OpenClaw Memory System Explained"
date: 2025-09-20
categories: [technical]
tags: [openclaw, memory, context, ai]
description: "How OpenClaw maintains continuity across sessions with its file-based memory system."
---

AI models are stateless — they forget everything between sessions. OpenClaw's memory system bridges this gap, giving your agent continuity and the ability to remember your preferences, history, and context.

## How Memory Works

OpenClaw uses a file-based memory system in the workspace:

```
~/.openclaw/workspace/
├── SOUL.md      — Agent's personality and behavior
├── MEMORY.md    — Long-term curated memories
├── memory/
│   ├── 2025-09-18.md  — Daily notes
│   ├── 2025-09-19.md  — Daily notes
│   └── 2025-09-20.md  — Today's notes
```

At the start of each session, the agent reads these files. They provide context about who you are, what's been happening, and what matters.

## Daily Notes vs Long-Term Memory

**Daily notes** (`memory/YYYY-MM-DD.md`) are raw logs. The agent writes what happened during each session: tasks completed, decisions made, things learned.

**MEMORY.md** is curated. The agent periodically reviews daily notes and distills the important stuff: your preferences, recurring topics, key decisions, lessons learned.

Think of it like a journal (daily notes) vs a personal wiki (MEMORY.md).

## What Gets Remembered

The agent decides what's worth remembering:

- Your communication preferences
- Important contacts and relationships
- Recurring tasks and schedules
- Decisions and their rationale
- Things you've explicitly asked it to remember
- Lessons from mistakes

## What Doesn't Get Remembered

- Sensitive data (passwords, financial details) — unless stored in the secure keychain
- Routine interactions that don't add context
- One-off questions with no ongoing relevance

## The Memory Maintenance Cycle

Periodically (usually during heartbeat checks), the agent:

1. Reviews recent daily notes
2. Identifies significant events and learnings
3. Updates MEMORY.md with distilled insights
4. Removes outdated information

This mirrors how human memory works: short-term → long-term, with natural forgetting of irrelevant details.

## Practical Impact

Without memory, every session starts from zero. Your agent wouldn't know:
- That you prefer morning briefings at 8am, not 7am
- That "John" refers to John Smith, your biggest client
- That you had a meeting about Project X yesterday
- That you hate email newsletters and want them auto-archived

With memory, your agent builds a progressively richer understanding of you and your world. After a month of use, the difference is stark.

## Privacy and Security

Memory files are stored locally in plaintext markdown. This means:
- Anyone with access to your machine can read them
- Back them up (they're irreplaceable)
- Review them periodically for sensitive information
- The agent only loads MEMORY.md in private sessions (not shared group chats)

---

*Memory configuration is part of my [setup service](/services/). I'll help you establish the right memory structure for your use case.*
