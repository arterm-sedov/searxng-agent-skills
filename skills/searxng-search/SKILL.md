---
name: searxng-search
description: |
  Use ONLY when the user explicitly wants a FREE, LOCAL, NO API KEY web search solution — says "free search", "no API key search", "local search", "self-hosted search", "private search", "SearXNG", "free alternative to Tavily", or mentions wanting search without API keys, rate limits, or paid services. Do NOT use for general web search — use tavily-search instead.
compatibility: Requires searxng-docker-tavily-adapter Docker stack running on localhost:8000
allowed-tools: Bash(curl *)
---

# searxng search

Free, unlimited web search via local SearXNG. No API key required.

## Prerequisites

Requires [searxng-docker-tavily-adapter](https://github.com/vakovalskii/searxng-docker-tavily-adapter) running locally.

```bash
# Check if running
curl -s http://localhost:8000/health
```

If not running, start the Docker stack in your adapter directory:
```bash
docker compose up -d
```

## When to use

- Need web search without API keys or limits
- User mentions "free", "no API key", "local", "private"
- Privacy-focused (local deployment)
- Drop-in Tavily replacement for Python code

## When NOT to use

- Need Tavily's AI-powered research synthesis → use tavily-research
- Need advanced search features (topic filtering, time ranges) → use tavily-search
- Docker unavailable and user won't start it → use tavily-search

## Quick start

```bash
curl -X POST "http://localhost:8000/search" \
  -H "Content-Type: application/json" \
  -d '{"query": "your query", "max_results": 5}'
```

## Options

| Option | Description |
|--------|-------------|
| `query` | Search query (required) |
| `max_results` | Max results 1-20 (default: 10) |
| `include_raw_content` | Include full page text (default: false) |

## Response

```json
{
  "query": "search query",
  "results": [{"url": "...", "title": "...", "content": "...", "score": 0.9}],
  "response_time": 1.23
}
```

## Tips

- **Without raw_content**: ~1-2s, snippets only
- **With raw_content**: ~3-5s, full page text (up to 2500 chars)
- **No API key needed** — completely free

## Python (drop-in Tavily replacement)

```python
from tavily import TavilyClient
client = TavilyClient(api_key="ignored", base_url="http://localhost:8000")
response = client.search(query="your query", max_results=5)
```

## See also

- [searxng-extract](../searxng-extract/SKILL.md) — extract from URLs
- [tavily-search](../tavily-search/SKILL.md) — original Tavily (requires API key)
