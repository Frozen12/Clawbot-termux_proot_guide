
# OpenClaw Setup Guide for Android
---

<p align="center">
  <img src="openclaw-dashboard.jpeg" width="45%" />
  <img src="termux-setup.jpeg" width="45%" />
</p>

## What You’re Building

By the end of this guide, your Android phone will:

- Run **OpenClaw locally**
- Act as a **24/7 AI agent**
- Be controllable from a **web dashboard**
- Operate **without a PC or cloud server**

---

## Requirements

Make sure you have:

- Android phone (**Android 10+ recommended**)
- Stable internet connection
- Gemini API key (from **Google AI Studio**)
- **Termux installed from F-Droid or GitHub** (not Play Store)

---

## Install Termux

1. Go to **F-Droid** or **GitHub**
2. Download **Termux v0.119.0-beta.3** or later version
3. Install and open Termux

> Older Termux versions may run `proot-distro` slowly on Android 10+.  
> Using the latest **Termux Beta** is recommended for better performance.

---

## Initial Termux Setup

```
pkg update && pkg upgrade -y
pkg install -y proot-distro
```

---

## Install Debian

```
proot-distro install debian
```

Login to Debian:

```
proot-distro login debian
```

---


## Setup Bash Shell

```
echo 'export SHELL=/bin/bash' >> ~/.bashrc
source ~/.bashrc
```
## Update Debian

```
apt update && apt upgrade -y
```

---

## Install Required Packages

```
apt install -y curl ca-certificates git nano
```

---

## Install Node.js LTS

```
curl -fsSL https://deb.nodesource.com/setup_lts.x | bash -
apt install -y nodejs
```

Verify installation:

```
node -v
```

---

## Enable pnpm

```
corepack enable
corepack prepare pnpm@latest --activate
```

Verify:

```
pnpm -v
```

---

## Setup pnpm Environment

Let pnpm configure its own environment:

```
pnpm setup
```

Reload shell:

```
source ~/.bashrc
```

---

## Install OpenClaw

```
pnpm add -g openclaw@latest
```

Verify:

```
openclaw --version
```

---

## Fix Android Network Interface Error

Some Android environments report incorrect network interfaces which can crash Node.js.

Create a small hijack script:

```
mkdir -p /root/.hijack
cat <<EOF > /root/.hijack/hijack.js
const os = require('os');
os.networkInterfaces = () => ({});
EOF
```

Load it automatically:

```
echo 'export NODE_OPTIONS="-r /root/.hijack/hijack.js"' >> ~/.bashrc
source ~/.bashrc
```

---

## Run OpenClaw Setup Wizard

```
openclaw onboard
```

When prompted for **Gateway Bind**, choose:

```
127.0.0.1
```

---

## Launch the OpenClaw Gateway

```
openclaw gateway --verbose
```

---

## Access the Web Dashboard

Open your browser and visit:

```
http://127.0.0.1:18789
```

Get your gateway token:

```
proot-distro login debian
cat ~/.openclaw/openclaw.json
```

or

```
openclaw config get gateway.auth.token
```

Paste the token into the dashboard login screen.

---

# Optional Tools & Integrations

These tools extend OpenClaw functionality.

---

## QMD – Query Markup Documents

Useful backend for OpenClaw memory systems.

```
pnpm add -g @tobilu/qmd
```

---

## OpenAI Codex CLI

Helpful for coding agents.

```
pnpm add -g @openai/codex
```

---

## Google Gemini CLI

Authenticate using a Google account instead of manually managing API keys.

```
pnpm add -g @google/gemini-cli
```

---

# Stability Tips

### Prevent Termux From Sleeping

```
termux-wake-lock
```

### Disable Battery Optimization

1. Open **Android Settings**
2. Apps → **Termux**
3. Battery
4. Disable optimization

### Keep Device Plugged In

For true **24/7 operation**, keep the phone connected to power.

---

# Security Tips

- Never share your **API keys**
- Never share your **gateway token**
- Use a **separate Google account** for AI services if possible

---

# What You Can Do Next

- Automate research tasks
- Build a personal AI assistant
- Connect it to messaging apps
- Use it as a mobile automation node
