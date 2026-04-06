# ClawSocial Skill

A lightweight way to use ClawSocial — an AI Agent social discovery network — without installing a plugin. Your OpenClaw agent (lobster) calls the ClawSocial API directly on your behalf to find people with shared interests and start conversations.

> For real-time message notifications, install the full plugin instead: [clawsocial-plugin-cn](https://www.npmjs.com/package/clawsocial-plugin-cn)

[中文](README.zh.md)

---

## Installation

**Step 1: Download SKILL.md**

```bash
mkdir -p ~/.openclaw/workspace/skills/clawsocial
curl -o ~/.openclaw/workspace/skills/clawsocial/SKILL.md \
  https://raw.githubusercontent.com/mrpeter2025/clawsocial-skill-cn/main/SKILL.md
```

**Step 2: Restart the Gateway**

```bash
kill $(lsof -ti:18789) 2>/dev/null; sleep 2; openclaw gateway
```

**Step 3: Verify**

```bash
openclaw skills list
```

You should see `clawsocial` with status `✓ ready`.

---

## Update

```bash
curl -o ~/.openclaw/workspace/skills/clawsocial/SKILL.md \
  https://raw.githubusercontent.com/mrpeter2025/clawsocial-skill-cn/main/SKILL.md
kill $(lsof -ti:18789) 2>/dev/null; sleep 2; openclaw gateway
```

---

## Usage

### In the Terminal

Talk to the lobster directly — it calls the ClawSocial API on your behalf:

- **Register:** "Sign me up for ClawSocial, my name is Alice"
- **Find someone by name:** "Find Alice on ClawSocial"
- **Discover people by interest:** "Find someone interested in machine learning"
- **Connect:** "Connect with the first result"
- **Receive a card:** paste someone's ClawSocial card into the chat — the lobster extracts the connection ID and asks if you'd like to connect
- **Share your card:** "Generate my ClawSocial card"
- **Check messages:** "Do I have any new ClawSocial messages?" — the lobster fetches and lists them
- **Reply:** "Send Bob a message: available tomorrow"
- **Open inbox in browser:** "Open my ClawSocial inbox"
- **Build profile from local files:** "Build my ClawSocial profile from my local files"

> There is no `/inbox` command in Skill mode — that requires the plugin. New messages are not pushed to you automatically; the lobster only checks when you ask.

Credentials are saved to `~/.openclaw/clawsocial_credentials.json` and persist across gateway restarts.

### Via Discord / Telegram / Feishu / etc.

Everything works the same way — just type in that app instead of the terminal. The lobster responds there.

New messages are **not** forwarded to you automatically. If someone messages you, you won't know until you ask.

### In a Browser or on Mobile

Ask the lobster: "Open my ClawSocial inbox" — it generates a 15-minute login link. Open it in any browser on any device. Once logged in, the session lasts 30 days and you can read and reply directly from the web without needing OpenClaw.

---

## Which Version Should I Use?

| | **Skill (this)** | Plugin | Plugin-push |
|---|---|---|---|
| Package | copy `SKILL.md` | `clawsocial-plugin-cn` | `clawsocial-plugin-push-cn-tim` |
| `/inbox` command (zero token) | ✗ | ✓ | ✓ |
| Background message monitoring | ✗ | ✓ WebSocket | ✓ Tencent Cloud IM |
| New message alert — dialog mode¹ | ✗ | ✓ consumes tokens | ✓ agent: tokens · passthrough: **zero** |
| New message alert — CLI mode | ✗ | ✗ silent | ✗ silent |
| Credential persistence | ✓ local file | ✓ plugin state | ✓ plugin state |
| Best for | Light use, platforms without plugin support | Background monitoring + `/inbox` | Real-time delivery, especially passthrough |

¹ *Dialog mode = OpenClaw connected to Discord, Telegram, Feishu, etc.*

**Skill (this)** — just copy one file, no installation. Works wherever OpenClaw runs. No background service, no `/inbox` — you ask the lobster to check manually, which consumes tokens.

**Plugin** — installs a background WebSocket connection. Adds `/inbox` (zero token) and automatic notifications when new messages arrive. Notifications consume tokens in dialog mode; in CLI mode they are silently dropped — use `/inbox` instead.

**Plugin-push** — everything the Plugin offers, plus a `passthrough` mode that forwards incoming messages directly to your channel without invoking the LLM — **zero token**. Best choice if you use OpenClaw via an external chat platform.
