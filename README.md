# OpenClaw Setup Guide for Android 
---

<p align="center">
  <img src= openclaw-dashboard.jpeg width="45%" />
  <img src= termux-setup.jpeg width="45%" />
</p>

## What You’re Building

By the end of this guide, your Android phone will:

- Run OpenClaw locally
- Act as a 24/7 AI agent
- Be controllable from a web dashboard
- Operate without a PC or cloud server

---

## Requirements

Make sure you have:

- Android phone (Android 10 or above recommended)
- Stable internet connection
- Gemini API key (from Google AI Studio)
- Termux installed from F-Droid or github (not Play Store)

---

## Install Termux

1. Go to **F-Droid.org** or **github**
2. Download and install **Termux_v0.119.0-beta.3**
3. Open the Termux app

---

## Commands

```
pkg update && pkg upgrade -y

```
## install proot-distro
```
pkg install proot-distro

```

## install debian
```
proot-distro install debian

```

## login debian
```
proot-distro login debian

```
## Update system
```
apt update && apt upgrade -y

```

## Install Dependencies
```
apt install -y curl ca-certificates nano fish

```
## Change Default Shell to Fish
```
chsh -s /usr/bin/fish

```
=/> Now restart your shell or login again.

## Install Node.js LTS (System-wide)

Run the following command to install **Node.js LTS**:

```
curl -fsSL https://deb.nodesource.com/setup_lts.x | bash - && apt install -y nodejs
```

This installs:

- Node.js LTS
- npm
- system-wide binaries

---

## Enable pnpm with Corepack

```
corepack enable
corepack prepare pnpm@latest --activate
```

---

## Verify Installation
```
node -v
pnpm -v
```
## update and install git
```
apt update
apt install -y git

```


##  Install OpenClaw
```
pnpm add -g openclaw@latest

```
After installation, check:

```
openclaw --version

```
---

## Fix Android Network Interface Error

Create the **hijack script**:


```
cat <<EOF > /root/hijack.js
const os = require('os');
os.networkInterfaces = () => ({});
EOF
```

Make it load automatically:


```
echo 'export NODE_OPTIONS="-r /root/hijack.js"' >> ~/.bashrc
source ~/.bashrc
```

---

## Run OpenClaw Setup Wizard

Start onboarding:


```
openclaw onboard
```

When prompted for **Gateway Bind**, select:



127.0.0.1 (Loopback)


---

## Launch the OpenClaw Gateway

Start the agent:


```
openclaw gateway --verbose
```

---

## Access the Web Dashboard

Open your mobile browser and go to:


```
http://127.0.0.1:18789

```
Get your gateway token:
start new terminal session
login ubuntu
run -
```
cat ~/.openclaw/openclaw.json
```
openclaw config get gateway.auth.token


Paste the token into the dashboard login screen.

---

## Useful Agent Commands



/status

Check agent health.



/think high

Enable deep reasoning mode.



/reset

Clear memory and restart the session.

---


# Optional Tools & Integrations

These tools are **not required**, but they are useful when working with **OpenClaw agents**.

---

# QMD - Query Markup Documents

Useful backend for Openclaw memory keeping

```
pnpm add -g @tobilu/qmd

```

# OpenAI Codex CLI

Useful for accessing openAI models without api

```
pnpm add -g @openai/codex
```
# Google Gemini CLI

Allows using **Gemini models without API keys**.

```
pnpm add -g @google/gemini-cli
```
---

## Stability Tips

### Prevent Termux from Sleeping



termux-wake-lock


### Disable Battery Optimization

1. Go to Android Settings
2. Apps → Termux
3. Battery
4. Disable optimization

### Keep Device Plugged In

For true 24/7 operation, keep the phone connected to power.

---

## Security Tips

- Never share your API keys publicly
- Do not share your gateway token
- Use a separate Google account for AI keys if possible

---

## What You Can Do Next

- Automate research tasks
- Build a personal AI assistant
- Connect it to messaging apps
- Use it as a mobile automation node

---


