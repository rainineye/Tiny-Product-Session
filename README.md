# Tiny Product Session 🐧

A local-first, browser-based multi-agent workspace. Route messages to four AI agents simultaneously — Anthropic and OpenAI — and get all responses in a single unified thread. No backend required for Anthropic; OpenAI routes through a lightweight Cloudflare Worker proxy.

**Live:** [rainineye.github.io/Tiny-Product-Session](https://rainineye.github.io/Tiny-Product-Session/)

---

## Agents

| Agent | Model | Role |
|-------|-------|------|
| `@brain` | claude-opus-4-6 | Strategy · architecture · decisions |
| `@thinker` | gpt-5.4 | Deep reasoning · first principles |
| `@coder` | claude-sonnet-4-6 | Code · review · implementation |
| `@codex` | o3 | Cross-check · algorithmic reasoning |

All agents are declared multi-agent session-aware in their system prompts — they know each other exists and can engage directly.

---

## Features

### Routing
- Type freely → all four agents reply in parallel
- Use `@brain`, `@thinker` etc. to address specific agents
- Toggle agents on/off in the route bar
- Click a 🐧 icon in the top-left to filter the thread by agent

### @mention autocomplete
Type `@` in the message box → dropdown with all agents. Navigate ↑↓, confirm with Enter or Tab.

### ⟳ Discuss mode
After `@brain` and `@thinker` have both responded, press **⟳ discuss**. They run sequentially — each reads the other's last reply, injected into the API payload only (not stored in history), so the thread stays clean.

### ❝ Quote & route
Hover (desktop) or tap (mobile) any agent message → **❝ quote** inserts an excerpt into the input. Add an `@mention` to forward it to a specific agent.

### Attachments
📎 inside the message box, or drag and drop. Supports images (vision), `.md`, `.txt`, `.json`, `.csv`.

### Session I/O
- **↓ export** → download session as `.md`
- **↑ import** → upload a previous `.md` to restore context

### API Key management
⚙ agents → API KEYS. Enter directly or save/load AES-256-GCM encrypted `.tpok` key file.

---

## Setup

### 1. API Keys
Open ⚙ agents and enter your Anthropic and/or OpenAI API keys. Stored in `localStorage` only.

### 2. OpenAI proxy (required for `@thinker` and `@codex`)

OpenAI blocks direct browser calls (CORS). Deploy a one-file Cloudflare Worker:

1. [dash.cloudflare.com](https://dash.cloudflare.com) → Workers & Pages → Create → Start with Hello World
2. Paste contents of [`worker.js`](./worker.js)
3. Deploy → copy your worker URL
4. In `index.html`, replace the `oaiUrl` constant with your worker URL

> The worker only proxies POST to `/v1/chat/completions`. No logging or storage.

---

## Stack

- Single HTML file, zero dependencies, vanilla JS
- Anthropic API — direct browser access via official header
- OpenAI API — via Cloudflare Worker proxy
- Web Crypto API — AES-256-GCM encrypted key storage
- localStorage — session persistence
- Responsive — desktop + mobile (iPhone 14 Pro tested)

---

## Local development

```bash
python -m http.server 8080
# → http://localhost:8080
```

---

## License

MIT
