---
layout: post
title: "OpenClaw on Raspberry Pi: Complete Guide"
date: 2025-01-12
categories: [setup, guide]
tags: [openclaw, raspberry-pi, installation]
description: "Run your own AI agent on a $35 Raspberry Pi. Complete setup guide for OpenClaw on ARM hardware."
---

Running a personal AI agent doesn't require expensive hardware. A Raspberry Pi 4 or 5 can handle OpenClaw perfectly well, and the whole setup costs under $100. Here's the complete guide.

## Hardware You'll Need

- Raspberry Pi 4 (4GB+) or Pi 5
- 32GB+ microSD card (or USB SSD for better performance)
- Power supply
- Ethernet cable (recommended over WiFi for reliability)

Total cost: $50-80 depending on what you already have.

## Installing the OS

Flash Raspberry Pi OS Lite (64-bit) using the Raspberry Pi Imager. The Lite version is headless — no desktop environment wasting resources.

During the flash process, enable SSH and set your username/password. You'll do everything remotely from here.

## Setting Up Node.js

The default Node.js in Raspberry Pi OS repos is ancient. Use NodeSource:

```bash
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt-get install -y nodejs
```

Verify: `node --version` should show v20+.

## Installing OpenClaw

```bash
sudo npm install -g openclaw
openclaw gateway start
```

The first start takes a moment as it creates the workspace directory and default configs.

## Performance Tips for Pi

The Pi isn't a powerhouse, so optimize:

1. **Use lighter models** — Claude Haiku or GPT-4o-mini respond faster and cost less
2. **Disable browser automation** — it's resource-heavy and rarely needed on a Pi
3. **Use an SSD** — microSD cards are slow for the kind of random I/O OpenClaw does
4. **Allocate swap** — 2GB of swap helps prevent OOM kills

```bash
sudo fallocate -l 2G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
echo '/swapfile swap swap defaults 0 0' | sudo tee -a /etc/fstab
```

## Auto-Start on Boot

Create a systemd service so OpenClaw survives reboots:

```bash
sudo tee /etc/systemd/system/openclaw.service << EOF
[Unit]
Description=OpenClaw Gateway
After=network.target

[Service]
Type=simple
User=pi
ExecStart=/usr/bin/openclaw gateway start --foreground
Restart=always
RestartSec=10

[Install]
WantedBy=multi-user.target
EOF

sudo systemctl enable openclaw
sudo systemctl start openclaw
```

## Security Considerations

A Pi running 24/7 on your network needs basic hardening:

- Change the default password
- Enable UFW firewall: `sudo ufw allow ssh && sudo ufw enable`
- Keep the OS updated: `sudo apt update && sudo apt upgrade -y`
- Run `npx openclaw-audit` regularly

## Is the Pi Enough?

For a single-user AI agent handling Telegram messages, email triage, and calendar management? Absolutely. The AI processing happens in the cloud (via API calls to Claude, GPT, etc.) — the Pi just orchestrates. It's like a tiny, silent, always-on control center.

Where it struggles: browser automation, heavy file processing, or running local AI models. For those, step up to a Mac Mini.

---

*Want a Pi setup done right? [Check out my setup service](/services/) — $250 gets you a fully configured, secured, and tested OpenClaw installation on any hardware.*
