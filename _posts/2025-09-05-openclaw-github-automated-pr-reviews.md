---
layout: post
title: "OpenClaw + GitHub: Automated PR Reviews"
date: 2025-09-05
categories: [technical]
tags: [openclaw, github, pr-review, automation, development]
description: "Use OpenClaw to automatically review pull requests, catch issues, and speed up your development workflow."
---

If you're a developer or manage a development team, code review is essential but time-consuming. OpenClaw can automate the first pass, catching common issues before a human reviewer even looks at the PR.

## The Setup

OpenClaw integrates with GitHub via the `gh` CLI and GitHub API:

```bash
# Make sure gh is installed and authenticated
gh auth login

# Enable the GitHub skill
openclaw skill enable github
```

## What Automated PR Reviews Look Like

When a new PR is opened, OpenClaw can:

1. Read the diff
2. Analyze code quality, style, and potential bugs
3. Check for security issues
4. Verify tests are included
5. Post a review comment with findings

Example review comment:

> **AI Review Summary:**
> - ✅ Code style is consistent
> - ⚠️ Missing error handling in `processPayment()` (line 47)
> - ⚠️ No tests added for the new endpoint
> - 🔒 SQL query on line 23 may be vulnerable to injection
> - 💡 Consider extracting the validation logic into a shared utility

## Setting Up Automatic Triggers

Configure OpenClaw to watch for new PRs:

```bash
openclaw cron add --schedule "*/15 * * * *" \
  --prompt "Check for new PRs in my-org/my-repo that haven't been reviewed yet. Review any new ones." \
  --channel telegram
```

Or use GitHub webhooks for instant notification.

## The Human + AI Review Workflow

This isn't about replacing human review. It's about making it faster:

1. **PR opened** → AI does first-pass review (2 minutes)
2. **AI posts findings** → Developer addresses obvious issues
3. **Human reviewer** sees a cleaner PR with fewer trivial issues
4. **Review time** drops from 30 minutes to 10 minutes

## What AI Catches Well

- Style inconsistencies
- Missing error handling
- Obvious security issues (SQL injection, XSS)
- Missing tests
- Dead code
- Documentation gaps
- Naming convention violations

## What Humans Still Need to Do

- Architecture decisions
- Business logic correctness
- Performance implications
- UX considerations
- Context-dependent trade-offs

The AI handles the mechanical stuff; humans handle the judgment calls.

---

*Want automated PR reviews for your team? [Contact me](/services/) — I'll set up the GitHub integration and configure review rules specific to your codebase.*
