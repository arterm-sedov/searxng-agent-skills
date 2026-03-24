---
name: searxng-search
description: |
  Use for web search when you need more than simple lookups AND want a free, unlimited solution. Priority: native websearch (simple) → searxng-search (comprehensive, free) → tavily-search (advanced features). Use when: user wants comprehensive results, mentions "free", "unlimited", "local", "no API key", "SearXNG", or searxng-docker-tavily-adapter stack is running. Falls back to tavily-search if advanced features needed (topic filtering, time ranges).
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

If not running, ask user for permission to set up:

```bash
# Clone and configure (with user permission)
git clone https://github.com/vakovalskii/searxng-docker-tavily-adapter
cd searxng-docker-tavily-adapter
cp config.example.yaml config.yaml
# Generate secret key and update config.yaml
python -c "import secrets; print(secrets.token_urlsafe(32))"
docker compose up -d
```

Or start existing stack: `docker compose up -d`

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
