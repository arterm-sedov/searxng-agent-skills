---
name: searxng-search
description: |
  Use when the user wants to search the web, find articles, look up information, get recent news, discover sources, or says "search for", "find me", "look up", "what's the latest on", "find articles about" — AND mentions "free", "no API key", "local", "private", "self-hosted", "without paying", or wants an alternative to Tavily. Free, unlimited web search via local Docker deployment.
compatibility: Requires Docker stack at D:/Repo/searxng-docker-tavily-adapter (run `docker compose up -d`)
allowed-tools: Bash(curl *)
---

# searxng search

Free, unlimited web search via local SearXNG. No API key required.

## Prerequisites Check

```bash
curl -s http://localhost:8000/health || echo "Docker stack not running"
```

If down: `cd D:/Repo/searxng-docker-tavily-adapter && docker compose up -d`

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
