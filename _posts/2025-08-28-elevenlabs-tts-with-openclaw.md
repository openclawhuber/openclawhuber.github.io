---
layout: post
title: "Using ElevenLabs TTS with OpenClaw"
date: 2025-08-28
categories: [technical]
tags: [openclaw, elevenlabs, tts, voice]
description: "Add natural-sounding voice output to your OpenClaw agent with ElevenLabs text-to-speech."
---

Text is great, but sometimes you want your AI agent to talk to you. ElevenLabs produces the most natural-sounding AI speech available, and OpenClaw integrates with it seamlessly.

## Why Voice?

- **Hands-free interaction** — Listen to briefings while commuting
- **Accessibility** — Voice is easier for some users than reading
- **Engagement** — Stories, summaries, and updates are more engaging as audio
- **Multitasking** — Listen while doing something else

## Setting Up ElevenLabs

1. Create an account at [elevenlabs.io](https://elevenlabs.io)
2. Get your API key from the dashboard
3. Configure in OpenClaw:

```bash
openclaw config set tts.provider elevenlabs
openclaw config set tts.apiKey "your-api-key"
openclaw config set tts.voice "rachel"  # or any ElevenLabs voice
```

## Using Voice in Practice

Once configured, your agent can send voice messages on platforms that support them (Telegram, Discord):

```
You: "Give me today's briefing as a voice message"
Agent: [sends audio message with your briefing]
```

You can also configure certain outputs to always be voice:

```yaml
tts:
  autoVoice:
    - morningBriefing
    - meetingReminders
    - storyTime
```

## Choosing a Voice

ElevenLabs offers dozens of pre-built voices and lets you clone custom ones. For a professional assistant:

- **Rachel** — Clear, professional female voice
- **Adam** — Warm, confident male voice
- **Charlie** — Casual, friendly male voice

Or clone your own voice (or any voice you have permission to use) for a truly personalized experience.

## Cost

ElevenLabs pricing:
- Free tier: 10,000 characters/month
- Starter: $5/month for 30,000 characters
- Creator: $22/month for 100,000 characters

A typical morning briefing is ~500 characters. At 30 briefings/month, you're well within the free tier. Add meeting reminders and occasional voice responses, and the $5/month tier covers most users.

## Fun Use Cases

Beyond practical briefings, voice adds personality:

- **Story time** — Have your agent narrate stories or summaries with dramatic flair
- **Language practice** — Listen to translations in natural-sounding speech
- **Audiobook-style** — Convert long articles or documents to audio for your commute
- **Funny voices** — Use different voices for different types of messages

---

*Want voice integrated into your AI setup? [My services](/services/) include TTS configuration with voice selection and routing rules.*
