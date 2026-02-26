---
layout: post
title: "Running OpenClaw on a VPS (DigitalOcean, Hetzner)"
date: 2025-02-18
categories: [setup, guide]
tags: [openclaw, vps, cloud, hosting]
description: "Host your AI agent on a VPS for reliable 24/7 uptime. Setup guide for DigitalOcean and Hetzner."
---

Not everyone has a Mac Mini sitting around. A VPS (Virtual Private Server) is the cloud-based alternative: reliable, always-on, and starting at $4/month.

## Choosing a Provider

**DigitalOcean** — Great UI, solid docs, $6/month for 1GB RAM droplet. US and EU regions.

**Hetzner** — Incredible value. €3.29/month for 2GB RAM in Germany or Finland. My top pick for European users.

**Vultr, Linode** — Also solid options in the $5-6/month range.

## Minimum Specs

OpenClaw itself is lightweight. You need:
- 1 vCPU
- 1GB RAM (2GB preferred)
- 20GB SSD
- Ubuntu 22.04 or 24.04

## Setup Steps

### 1. Create the server

Pick Ubuntu 24.04 LTS, the smallest instance that has 1GB+ RAM, and add your SSH key.

### 2. Initial server hardening

```bash
# Update everything
apt update && apt upgrade -y

# Create a non-root user
adduser openclaw
usermod -aG sudo openclaw

# Set up SSH key auth for the new user
mkdir -p /home/openclaw/.ssh
cp ~/.ssh/authorized_keys /home/openclaw/.ssh/
chown -R openclaw:openclaw /home/openclaw/.ssh

# Disable root login and password auth
sed -i 's/PermitRootLogin yes/PermitRootLogin no/' /etc/ssh/sshd_config
sed -i 's/#PasswordAuthentication yes/PasswordAuthentication no/' /etc/ssh/sshd_config
systemctl restart sshd

# Enable firewall
ufw allow ssh
ufw allow 443
ufw enable
```

### 3. Install Node.js and OpenClaw

```bash
su - openclaw
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt-get install -y nodejs
npm install -g openclaw
```

### 4. Start and enable

Use the systemd service approach (same as the Raspberry Pi guide) to keep OpenClaw running across reboots.

## VPS vs Home Hardware

**VPS advantages:**
- 99.9%+ uptime guaranteed
- No power outage worries
- Easy to scale up
- Professional networking (static IP, low latency)

**Home hardware advantages:**
- No monthly hosting cost
- Complete physical control
- Better for privacy (your machine, your house)
- No vendor dependency

## Cost Comparison

| Setup | Monthly Cost |
|-------|-------------|
| Mac Mini at home | ~$3 (electricity) |
| Raspberry Pi at home | ~$1 (electricity) |
| Hetzner VPS | €3.29 (~$3.50) |
| DigitalOcean VPS | $6 |

Add API costs ($5-20/month for model usage) to all options. The hosting itself is cheap no matter which route you pick.

---

*Want a VPS setup done right? [My $250 setup service](/services/) includes full server hardening, OpenClaw configuration, and channel integration. You'll have a production-ready AI agent by end of day.*
