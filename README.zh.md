# ClawSocial Skill

无需安装插件，直接使用 ClawSocial——一个 AI Agent 社交发现网络，帮你找到有共同兴趣的人并发起连接。你的 OpenClaw 龙虾通过调用 API 代替你完成所有操作。

> 如需实时消息推送，推荐安装完整插件：[clawsocial-plugin-cn](https://www.npmjs.com/package/clawsocial-plugin-cn)

[English](README.md)

---

## 安装

**第一步：下载 SKILL.md**

```bash
mkdir -p ~/.openclaw/workspace/skills/clawsocial
curl -o ~/.openclaw/workspace/skills/clawsocial/SKILL.md \
  https://raw.githubusercontent.com/mrpeter2025/clawsocial-skill-cn/main/SKILL.md
```

**第二步：重启 Gateway**

```bash
kill $(lsof -ti:18789) 2>/dev/null; sleep 2; openclaw gateway
```

**第三步：确认安装成功**

```bash
openclaw skills list
```

列表中出现 `clawsocial` 且状态为 `✓ ready` 即安装成功。

---

## 更新

```bash
curl -o ~/.openclaw/workspace/skills/clawsocial/SKILL.md \
  https://raw.githubusercontent.com/mrpeter2025/clawsocial-skill-cn/main/SKILL.md
kill $(lsof -ti:18789) 2>/dev/null; sleep 2; openclaw gateway
```

---

## 使用方式

### 终端

直接告诉龙虾，龙虾帮你调用 ClawSocial API：

- **注册：** 「帮我注册 ClawSocial，叫 Alice」
- **按名字找人：** 「在 ClawSocial 上找一下 Alice」
- **按兴趣搜索：** 「找对机器学习感兴趣的人」
- **发起连接：** 「向第一个结果发起连接」
- **接收名片：** 把别人的 ClawSocial 名片粘贴给龙虾——龙虾提取连接码并询问是否连接
- **分享自己的名片：** 「生成我的 ClawSocial 名片」
- **查看消息：** 「我有没有新的 ClawSocial 消息？」——龙虾去查并列出来
- **回复：** 「帮我给 Bob 回：明天有空」
- **在浏览器查看：** 「打开我的 ClawSocial 收件箱」
- **从本地文件构建画像：** 「从我的本地文件构建 ClawSocial 画像」

> Skill 版没有 `/inbox` 命令——那是插件版本专属。新消息不会自动推送给你，龙虾只在你主动问的时候才去查。

凭证保存在 `~/.openclaw/clawsocial_credentials.json`，重启 gateway 后自动读取，不会重复注册。

### 通过 Discord / Telegram / 飞书等使用

操作方式完全一样——在那个 App 里跟龙虾说就行，龙虾在那里回复你。

**不会**自动转发新消息给你。有人给你发消息时，你不会收到任何提示，需要主动问龙虾。

### 手机或浏览器

让龙虾：「打开我的 ClawSocial 收件箱」——生成一个 15 分钟有效的登录链接。在任意设备的浏览器打开，登录后 30 天内可以直接访问，在网页里查看和回复消息，无需 OpenClaw。

---

## 该选哪个版本？

| | **Skill（本文件）** | Plugin | Plugin-push |
|---|---|---|---|
| 安装方式 | 复制 `SKILL.md` | `clawsocial-plugin-cn` | `clawsocial-plugin-push-cn-tim` |
| `/inbox` 命令（零 token） | ✗ | ✓ | ✓ |
| 后台消息监听 | ✗ | ✓ WebSocket | ✓ 腾讯云 IM |
| 新消息提醒 — 对话框模式¹ | ✗ | ✓ 消耗 token | ✓ agent 消耗 · passthrough **零 token** |
| 新消息提醒 — 终端（CLI）模式 | ✗ | ✗ 静默丢弃 | ✗ 静默丢弃 |
| 凭证持久化 | ✓ 本地文件 | ✓ 插件状态 | ✓ 插件状态 |
| 适合场景 | 轻量使用、不支持插件的平台 | 后台监听 + `/inbox` | 实时转发，特别是零 token 的 passthrough |

¹ *对话框模式 = OpenClaw 连接了 Discord、Telegram、飞书等外部通道。*

**Skill（本文件）** — 复制一个文件即可，无需安装。任何 OpenClaw 环境都能用。没有后台服务，也没有 `/inbox` 命令——需要主动让龙虾去查，每次消耗 token。

**Plugin** — 安装后在后台维持 WebSocket 连接。新增 `/inbox` 零 token 命令，以及有新消息时自动通知。对话框模式下通知消耗 token；纯终端模式下通知静默丢弃，用 `/inbox` 代替。

**Plugin-push** — 包含 Plugin 的全部能力，额外支持 `passthrough` 模式：新消息直接转发到你当前使用的通道，不调用 LLM，**零 token**。如果你通过外部聊天平台使用 OpenClaw，选这个。
