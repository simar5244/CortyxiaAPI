# Cortyxia

[![npm](https://img.shields.io/npm/v/cortyxia)](https://www.npmjs.com/package/cortyxia)
[![PyPI](https://img.shields.io/pypi/v/cortyxia)](https://pypi.org/project/cortyxia/)
[![License](https://img.shields.io/badge/license-MIT-blue)](https://github.com/simar5244/CortyxiaAPI/blob/main/LICENSE)

> **The Memory Layer for Enterprise AI.**
> Drop-in persistent memory. 40% fewer tokens. One line to get started.

[Website](https://cortyxia.com) · [Docs](https://cortyxia.com/docs) · [PyPI](https://pypi.org/project/cortyxia/) · [npm](https://www.npmjs.com/package/cortyxia)

---

## Why Cortyxia?

Building LLM apps that "remember" across sessions shouldn't require a PhD in vector databases, embedding pipelines, or context-window math.

Cortyxia is a **drop-in memory layer** — swap your `openai.chat.completions.create()` for ours, and suddenly your app remembers everything. No schema design. No Pinecone setup. No chunking strategies. It just works.

Under the hood: a memory node infrastructure with multi-tier graph mechanisms (raw → indexed → entity-linked) and separate node architecture per tier. ONNX extractors, cross-encoder reranking, and semantic relevance scoring run at the proxy layer — sub-50ms routing overhead, zero prompt changes, fully compatible with whatever RAG or vector DB you're already running.

### What you get

- **Persistent memory** — conversations, facts, and context survive restarts
- **40% token cost reduction** — intelligent context assembly sends only what matters
- **Multi-provider routing** — OpenAI, Anthropic, Gemini, DeepSeek, xAI via one ISO token
- **Project isolation** — separate memory namespaces per team/project
- **Dev mode keys** — isolated context windows for agentic coding workflows

### What you DON'T need

- No vector DB setup (Pinecone, Weaviate, Chroma, etc.)
- No embedding pipeline
- No chunking / token-counting logic
- No config files to manage
- No signup forms — just an email

---

## Installation

**Python**

```bash
pip install cortyxia
```

**TypeScript / Node.js**

```bash
npm install cortyxia
```

---

## The 30-Second Test

Create a `.env` file:

```bash
CORTYXIA_EMAIL=you@example.com
API_KEY=your_provider_key
API_PROVIDER=openai
API_MODEL=gpt-4o
```

**Python**

```python
from cortyxia import Cortyxia

client = Cortyxia()
key = client.initialize()  # Creates project + key

resp = client.chat.completions.create(
    messages=[{"role": "user", "content": "My name is Alice"}]
)

# Later, new session:
resp2 = client.chat.completions.create(
    messages=[{"role": "user", "content": "What's my name?"}]
)
# It remembers.
```

**TypeScript**

```typescript
import { Cortyxia } from "cortyxia";

const client = await Cortyxia.create();
const key = await client.initialize();

const resp = await client.chat.completions.create({
  messages: [{ role: "user", content: "My name is Alice" }],
});

// Later, new session:
const resp2 = await client.chat.completions.create({
  messages: [{ role: "user", content: "What's my name?" }],
});
// It remembers.
```

No database connection strings. No index creation. One package. One line. Done.

---

## Migrating from OpenAI / LangChain / Raw HTTP

**Python**

```python
# Before
import openai
client = openai.OpenAI(api_key="sk-...")
resp = client.chat.completions.create(model="gpt-4o", messages=[...])

# After
from cortyxia import Cortyxia
client = Cortyxia()
resp = client.chat.completions.create(messages=[...])  # model auto-resolved
```

**TypeScript**

```typescript
// Before
import OpenAI from "openai";
const client = new OpenAI({ apiKey: "sk-..." });
const resp = await client.chat.completions.create({ model: "gpt-4o", messages: [...] });

// After
import { Cortyxia } from "cortyxia";
const client = await Cortyxia.create();
const resp = await client.chat.completions.create({ messages: [...] });
```

Your prompts don't change. Your message format doesn't change. The response shape is identical. If you don't like it, remove the import and go back. No lock-in.

---

## AI Coding Tools / CLI Agents

Cortyxia is built for the agentic coding revolution. Every major CLI agent and IDE extension can route through Cortyxia — giving your AI assistant **persistent memory** across sessions, **project isolation**, and **40% fewer tokens** without changing your workflow.

The pattern is the same everywhere: generate an ISO token from Cortyxia, point the tool at `https://app.cortyxia.com`, and set the token as the API key. The tool thinks it's talking to OpenAI/Anthropic. Cortyxia handles routing, memory injection, and context assembly in the background.

### Claude Code (Anthropic CLI)

Claude Code uses `ANTHROPIC_BASE_URL` and `ANTHROPIC_AUTH_TOKEN` (not `API_KEY`). You also need to override the model routing slots so Claude knows which model string to send:

```bash
# Clear any cached config
rm -rf ~/.claude/

# Point Claude at Cortyxia
export ANTHROPIC_BASE_URL="https://app.cortyxia.com"
export ANTHROPIC_AUTH_TOKEN="iso-your-token-here"

# Map Cortyxia model to Claude's internal routing slots
export ANTHROPIC_DEFAULT_OPUS_MODEL="choose-your-model"
export ANTHROPIC_DEFAULT_SONNET_MODEL="choose-your-model"
export ANTHROPIC_DEFAULT_HAIKU_MODEL="choose-your-model"

# Suppress client-side tracking probes (optional)
export CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC=1
export CLAUDE_CODE_DISABLE_EXPERIMENTAL_BETAS=true

# Launch
npx claude --model choose-your-model
```

Claude Code gets:
- **Session memory** — remembers files you edited, decisions you made, bugs you fixed
- **Dev mode isolation** — each conversation gets its own context window, no pollution
- **Multi-provider fallback** — swap the ISO token, same CLI

### Codex (OpenAI CLI)

Codex OSS uses a `config.toml` with a local bridge provider:

```bash
mkdir -p ~/.codex
cat << 'EOF' > ~/.codex/config.toml
oss_provider = "local_bridge"

[model_providers.local_bridge]
name = "Cortyxia Bridge"
base_url = "https://app.cortyxia.com/v1/"
env_key = "OPENAI_API_KEY"
wire_api = "responses"
requires_openai_auth = false
EOF

# Export your ISO token and launch
export OPENAI_API_KEY="iso-your-token-here"
codex --oss --config model='"choose-your-model"'
```

Codex now remembers across invocations — `codex "fix the auth bug from yesterday"` actually knows what you mean.

### VS Code Extensions (Roo Code, Continue, Cline, Kilo Code, etc.)

Most VS Code AI extensions support an **OpenAI Compatible** custom provider:

1. Settings → API Provider → **OpenAI Compatible** (or Custom)
2. **Base URL:** `https://app.cortyxia.com/v1/`
3. **API Key:** your ISO token (`iso-...`)
4. **Model:** the model name configured in your Cortyxia key (e.g. `claude/fable`)
5. **Streaming:** turn off if you see timeout errors on large outputs (works reliably with 30k+ token limits when streaming is disabled)

**Works with:** Roo Code, Continue.dev, Cline, Kilo Code, and any extension with a custom OpenAI-compatible provider setting.

### Cursor

Cursor supports custom OpenAI-compatible endpoints:

1. Settings → Models → Add Model
2. Base URL: `https://app.cortyxia.com/v1/`
3. API Key: your ISO token
4. Model: the model ID from your Cortyxia key

Cursor's Composer and Chat tabs now remember your project context across restarts.


### Base URL Cheat Sheet

| Tool | Base URL |
|------|----------|
| Claude Code | `https://app.cortyxia.com` (no `/v1`) |
| Codex | `https://app.cortyxia.com/v1/` |
| OpenAI-compatible tools | `https://app.cortyxia.com/v1/` |
| VS Code extensions | `https://app.cortyxia.com/v1/` |

### Why This Matters for Agentic Coding

Most CLI agents start from zero every session. Cortyxia fixes that:

- **Yesterday's debugging session** — remembered, referenced, built upon
- **Architecture decisions** — stored as indexed memory nodes, recalled when relevant
- **File relationships** — entity-linked graph memory knows `auth.rs` touches `user.ts`
- **Context window efficiency** — dev mode keys get only the most relevant memories, not a firehose

Each tool gets an isolated memory namespace (via `devNamespace`) so your Claude Code agent for API design doesn't pollute your Cursor agent for frontend bugs.

---

## API Overview

### Chat

```python
# Python
resp = client.chat.completions.create(messages=[...])
```

```typescript
// TypeScript
const resp = await client.chat.completions.create({ messages: [...] });
```

### Memory

```python
# Python
client.memory.add("User prefers Rust", ["preference"])
results = client.memory.query("What language?", limit=5)
```

```typescript
// TypeScript
await client.memory.add("User prefers Rust", ["preference"]);
const results = await client.memory.query("What language?", 5);
```

### Projects

```python
# Python
projects = client.projects.list()
project = client.projects.create("Agentic Coding", shared_memory_enabled=True)
```

```typescript
// TypeScript
const projects = await client.projects.list();
const project = await client.projects.create("Agentic Coding", true);
```

### Keys

```python
# Python
key = client.keys.create(
    project_id=client.project_id,
    label="Production",
    provider="openai",
    model="gpt-4o",
    provider_key="sk-..."
)
```

```typescript
// TypeScript
const key = await client.keys.create(client.projectId!, {
  label: "Production",
  provider: "openai",
  model: "gpt-4o",
  openaiKey: "sk-...",
});
```

Full API reference: [cortyxia.com/docs](https://cortyxia.com/docs)

---

## SDKs

| SDK | Install | Docs |
|-----|---------|------|
| Python | `pip install cortyxia` | [PyPI](https://pypi.org/project/cortyxia/) |
| TypeScript | `npm install cortyxia` | [npm](https://www.npmjs.com/package/cortyxia) |

---

## FAQ

**"Do I need to redesign my app architecture?"**

No. If you're already calling `chat.completions.create()`, you change the import. That's it. Memory happens automatically in the background.

**"What about my existing vector DB / RAG pipeline?"**

You can keep it. Cortyxia handles the "I talked to this user 3 days ago" memory layer. Your RAG pipeline handles the "here are the docs" layer. They complement each other.

**"How much does this cost?"**

Generous free tier — try it out or run small projects at no cost. You only pay for your LLM provider usage. And because Cortyxia assembles smarter context, you typically use **fewer tokens** — so your bill goes down, not up.

**"Is my data safe?"**

- Credentials stored locally with `0600` (Python) / `0o600` (Node) permissions
- Each email has fully isolated project data
- No third-party analytics or telemetry

**"Can I use this in production?"**

Yes. The core routing and memory layers are battle-tested. Pin your version:
- Python: `pip install cortyxia==0.1.16`
- TypeScript: `npm install cortyxia@0.1.13`

---

## License

MIT
