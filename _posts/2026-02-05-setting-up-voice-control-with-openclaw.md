---
layout: post
title: "Setting Up Voice Control with OpenClaw"
date: 2026-02-05
categories: [tutorial]
tags: [openclaw, voice, tts, speech, tutorial]
description: "Control your AI agent with voice messages and get spoken responses. Setup guide for voice-first interaction."
---

Typing is fine. But sometimes you want to talk to your AI agent — while driving, cooking, or just because it's faster. Here's how to set up voice-first interaction.

## Voice Input (You → Agent)

### Telegram Voice Messages

The easiest voice input method: send a voice message on Telegram. OpenClaw automatically:
1. Receives the audio
2. Transcribes it using Whisper (OpenAI's speech-to-text)
3. Processes the text as a normal message
4. Responds normally (text or voice)

No setup needed — it works out of the box.

### Tips for Voice Input

- Speak clearly, but don't over-enunciate (it's surprisingly good with natural speech)
- Pause briefly between sentences
- Spell out unusual names or technical terms if they're misheard
- "Hey, can you check my calendar for tomorrow" works perfectly

## Voice Output (Agent → You)

### ElevenLabs TTS

For natural-sounding voice responses:

```bash
openclaw config set tts.provider elevenlabs
openclaw config set tts.apiKey "your-key"
openclaw config set tts.defaultVoice "rachel"
```

### Configuring When to Use Voice

Not every response should be a voice message. Configure rules:

```yaml
tts:
  autoVoice:
    patterns:
      - "morning briefing"
      - "read me"
      - "tell me"
    channels:
      - telegram
    maxLength: 500  # Don't voice-narrate War and Peace
```

You can also explicitly request voice: "Read me today's schedule" vs "What's on my schedule today" (text response).

## Voice-First Workflows

### Morning Commute Briefing

Set up a cron job that sends a voice briefing right when you usually leave:

```
Every weekday at 7:45am:
- Today's weather
- Calendar overview
- Urgent emails summary
- Traffic conditions

Send as voice message on Telegram.
```

Listen during your commute. No screen needed.

### Hands-Free Task Management

While cooking, exercising, or driving:
- Voice note: "Add milk to the grocery list"
- Voice note: "Remind me to call the dentist tomorrow at 10"
- Voice note: "Draft an email to Sarah saying I'll be 15 minutes late"

Your agent handles each request and confirms with a short voice or text response.

### Meeting Prep Audio

Before meetings, get an audio briefing:
- Last meeting notes with this person
- Recent email context
- Prepared talking points

Listen while walking to the meeting room.

## Voice Quality Options

**ElevenLabs** — Best quality, most natural. ~$5-22/month depending on usage.

**macOS say** — Free, built-in, robotic but functional. Good for testing.

**OpenAI TTS** — Good quality, pay per character. Competitive pricing.

Choose based on how much you'll use voice. For daily briefings only, the free tier of ElevenLabs suffices.

## The Full Voice Loop

The dream setup: you speak to your agent, it speaks back. No typing, no reading, no screens.

It's not quite Star Trek — there are still delays and occasional misunderstandings. But for many daily tasks, it's genuinely useful and feels like the future.

---

*Want voice set up properly? [My setup service](/services/) includes TTS configuration and voice workflow design. Talk to your AI agent, literally.*
