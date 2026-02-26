---
layout: post
title: "How to Create Custom OpenClaw Skills"
date: 2025-08-05
categories: [technical, tutorial]
tags: [openclaw, skills, development, customization]
description: "Build custom skills to extend your OpenClaw agent's capabilities. Step-by-step guide with examples."
---

OpenClaw's skill system is what makes it infinitely extensible. If you can write a script, you can teach your AI agent to do anything. Here's how.

## Skill Structure

A skill is a directory with at minimum a `SKILL.md` file:

```
my-skill/
├── SKILL.md          # Instructions for the AI
├── scripts/          # Executable scripts
│   └── do-thing.sh
└── config.json       # Optional configuration
```

## Writing SKILL.md

This is the most important file. It tells the AI:
- What the skill does
- When to use it
- How to use it
- What tools/scripts are available

Example for a simple weather skill:

```markdown
# Weather Skill

## When to Use
Use this skill when the user asks about weather, temperature, or forecasts.

## How to Use
Run the weather script with a location:

\`\`\`bash
bash /path/to/skills/weather/scripts/get-weather.sh "San Francisco"
\`\`\`

The script returns current conditions and a 3-day forecast.

## Notes
- Supports any city name or zip code
- Data comes from wttr.in (no API key needed)
- Update frequency: real-time
```

## Writing the Script

```bash
#!/bin/bash
# scripts/get-weather.sh
LOCATION="${1:-San Francisco}"
curl -s "wttr.in/${LOCATION}?format=3"
```

## Installing Your Skill

Place the skill directory in OpenClaw's skills folder:

```bash
cp -r my-skill ~/.openclaw/skills/
```

Or use the ClawHub CLI to manage skills:

```bash
clawhub install my-skill
```

## A Real Example: Invoice Tracker

Here's a more practical skill I built for a client:

**SKILL.md:**
```markdown
# Invoice Tracker

Track and manage client invoices.

## Commands
- `invoice add <client> <amount> <due-date>` — Add a new invoice
- `invoice list` — Show all pending invoices  
- `invoice paid <id>` — Mark an invoice as paid
- `invoice overdue` — Show overdue invoices

## Data
Invoices are stored in ~/.openclaw/workspace/invoices.json
```

**scripts/invoice.sh:**
```bash
#!/bin/bash
ACTION=$1
INVOICES=~/.openclaw/workspace/invoices.json

case $ACTION in
  add)
    # Add invoice logic using jq
    ;;
  list)
    cat $INVOICES | jq '.[] | select(.status == "pending")'
    ;;
  paid)
    # Mark as paid logic
    ;;
  overdue)
    cat $INVOICES | jq '.[] | select(.due < now and .status == "pending")'
    ;;
esac
```

## Tips for Good Skills

1. **Clear SKILL.md** — The AI can only use your skill if it understands the instructions
2. **Error handling** — Scripts should fail gracefully with helpful messages
3. **Idempotent operations** — Running a command twice shouldn't break things
4. **Minimal dependencies** — Don't require exotic packages
5. **Test manually first** — Make sure the scripts work before letting the AI use them

## Sharing Skills

Built something useful? Publish it to ClawHub for others:

```bash
clawhub publish my-skill
```

The community benefits, and you build reputation.

---

*Need a custom skill built for your workflow? [Contact me](/services/) — I'll design and implement skills tailored to your specific business needs.*
