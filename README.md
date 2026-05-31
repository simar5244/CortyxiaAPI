<div align="center">

# Cortyxia

**The Memory Layer for Enterprise AI**

[![npm version](https://badge.fury.io/js/cortyxia.svg)](https://www.npmjs.com/package/cortyxia)
[![PyPI version](https://badge.fury.io/py/cortyxia.svg)](https://pypi.org/project/cortyxia/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

One API swap adds persistent memory, observability, and 40% token cost reduction to any LLM stack.

---

</div>

## What is Cortyxia?

Cortyxia is a memory layer that sits between your application and any LLM provider (Anthropic, Google, DeepSeek, xAI, Llama, Groq, and more). It wraps around your existing API workflow and adds:

- **Persistent memory** — Every conversation enriches a shared knowledge base that compounds over time
- **Zero-overhead capture** — Use any AI exactly as you do today; context is automatically captured and structured at the infrastructure layer
- **Precision retrieval** — BM25 + semantic reranking surfaces exactly the memory your model needs, keeping prompts lean
- **Model-agnostic** — Switch providers instantly without changing your code
- **Private memory keys** — Every project gets isolated context that cannot cross-contaminate
- **OSuite Observability** — Deep AI observability with model comparison, prompt metrics, and guardrail checks

## Installation

### TypeScript / JavaScript

```bash
npm install cortyxia
```

```typescript
import { Cortyxia } from "cortyxia";

const client = new Cortyxia({
  isoUrl:  "https://proxy.cortyxia.com",
  isoToken: "iso-...",  // Your Cortyxia project token
});

// Automatic memory injection — zero code changes beyond this
const res = await client.chat.completions.create({
  messages: [{ role: "user", content: "What did we discuss yesterday?" }],
});

console.log(res.choices[0].message.content);
```

### Python

```bash
pip install cortyxia
```

```python
from cortyxia import Cortyxia

client = Cortyxia(
    iso_url="https://proxy.cortyxia.com",
    iso_token="iso-...",  # Your Cortyxia project token
)

# Automatic memory injection — zero code changes beyond this
res = client.chat.completions.create(
    messages=[{"role": "user", "content": "What did we discuss yesterday?"}]
)

print(res["choices"][0]["message"]["content"])
```

## How It Works

1. You send a message through the SDK (or any HTTP client)
2. Cortyxia retrieves relevant memory from your project's knowledge base
3. Retrieved context is injected into the prompt
4. Request is forwarded to your configured provider
5. Response is captured and scored for future retrieval

## Features

### Cumulative Intelligence
Every resolved ticket, strategic decision, and customer conversation enriches your shared memory. New team members inherit years of knowledge on day one.

### Token-Efficient Memory
Smart routing and semantic caching reduces total token usage 40-60%. Hot nodes rank higher, stale context gets deprioritized, and prompts stay lean.

### Cross-Platform Sync
Real-time bidirectional synchronization keeps every connected app in lockstep. Native integrations with Salesforce, HubSpot, Zendesk, ServiceNow, Jira, Slack, Teams, and more.

### Drop-in Compatibility
No refactoring required. Import, add your token, and keep using your existing code.

## Configuration

### TypeScript

```typescript
const client = new Cortyxia({
  isoUrl:     "https://proxy.cortyxia.com",  // Required
  isoToken:   "iso-...",                     // Required — your project token
  timeout:    60000,                         // Optional — ms (default: 60000)
});
```

### Python

```python
client = Cortyxia(
    iso_url="https://proxy.cortyxia.com",   # Required
    iso_token="iso-...",                     # Required — your project token
    timeout=60,                               # Optional — seconds (default: 60)
)
```

## Advanced Usage

### Memory Seeding (Bulk Import)

**TypeScript:**
```typescript
await client.memory.add("User is a vegetarian", ["diet", "preference"]);
```

**Python:**
```python
client.memory.add("User is a vegetarian", tags=["diet", "preference"])
```

### Direct Memory Query

**TypeScript:**
```typescript
const hits = await client.memory.query("vegetarian preferences", 5);
```

**Python:**
```python
hits = client.memory.query("vegetarian preferences", limit=5)
```

## Verified Results

- **40%** token cost reduction at Texas Tech Online
- **2.3M tokens/day** production workload baseline
- **Millions of messages** processed in 2-week pilot
- **100% traceability** across all interactions

## Links

- **Website:** https://www.cortyxia.com
- **Documentation:** https://docs.cortyxia.com
- **GitHub:** https://github.com/simar5244/CortyxiaAPI


---

<div align="center">

**The Memory Layer for Enterprise AI**

Made with ❤️ by [Cortyxia](https://www.cortyxia.com)

</div>
