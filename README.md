# Tiny Product Session 🐧

A local-first, browser-based multi-agent workspace. Route your messages to four AI agents simultaneously — from Anthropic and OpenAI — and get all responses in a single unified thread.

![four penguins](https://img.shields.io/badge/agents-4-black) ![anthropic](https://img.shields.io/badge/Anthropic-Opus%20%2F%20Sonnet-e8631a) ![openai](https://img.shields.io/badge/OpenAI-GPT--5.4%20%2F%20o3-38bdf8)

---

## Agents

| Agent | Model | Role |
|-------|-------|------|
| `@brain` | claude-opus-4-6 | Strategy · architecture · decisions |
| `@thinker` | gpt-5.4 | Deep reasoning · first principles |
| `@coder` | claude-sonnet-4-6 | Code · review · implementation |
| `@codex` | o3 | Cross-check · algorithmic reasoning |

**Thinkers** (`@brain` + `@thinker`) can debate each other via **⟳ discuss** mode — each reads the other's last response and replies directly.

---

## Usage

### Routing
- Type freely → all four agents reply
- `@brain what's the architecture tradeoff here?` → only `@brain` replies
- Use the route bar to toggle which agents receive each message
- Click a 🐧 icon in the top-left to filter the thread by agent

### Discuss mode
After both `@brain` and `@thinker` have responded, press **⟳ discuss** in the route bar. They'll read each other's last message and debate directly. Press again for another round.

### Attachments
Click the **📎 Attach File** button inside the message box, or drag and drop files. Supports images (sent via vision API), `.md`, `.txt`, `.json`, `.csv`.

### Session I/O
- **↓ export** → name and download the session as a `.md` file (Obsidian-compatible)
- **↑ import** → restore a previous session's context from an exported `.md` file

---

## Setup

### 1. API Keys
Open **⚙ agents** → enter your Anthropic and OpenAI API keys. Keys are stored in `localStorage` — never sent anywhere except directly to the APIs.

You can also save keys as an encrypted `.tpok` file (AES-256-GCM) and load them next time without re-typing.

### 2. Deploy your own OpenAI proxy (required for `@thinker` and `@codex`)

OpenAI blocks direct browser API calls. You need a one-file Cloudflare Worker as a proxy.

**Deploy the worker:**
1. Go to [dash.cloudflare.com](https://dash.cloudflare.com) → Workers & Pages → Create → Start with Hello World
2. Replace the default code with the contents of [`worker.js`](./worker.js)
3. Deploy → copy your worker URL (e.g. `https://tpo-proxy.yourname.workers.dev`)

**Update the HTML:**
In `index.html`, find:
```js
const oaiUrl='https://YOUR_WORKER_URL/v1/chat/completions';
```
Replace with your worker URL.

> The worker only proxies POST requests to `/v1/chat/completions`. It does not log, store, or inspect your API keys or messages.

---

## Stack

- Zero dependencies — single HTML file, vanilla JS
- Anthropic API (direct browser access via official header)
- OpenAI API (via Cloudflare Worker proxy)
- Web Crypto API for encrypted key storage
- localStorage for session persistence
- Pixel art penguins 🐧

---

## Local development

```bash
# No build step needed — just open the file
open index.html

# Or serve locally (recommended for file:// CORS reasons)
python -m http.server 8080
# then open http://localhost:8080
```

---

## License

MIT
