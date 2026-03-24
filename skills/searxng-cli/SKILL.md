---
name: searxng-cli
description: |
  Use when the user wants web search, content extraction, or says "free alternative to Tavily", "self-hosted search", "local search API", "no API key", "private search", "SearXNG" — AND wants a free, unlimited, local alternative to Tavily with no API keys or rate limits. Provides search and extraction via local Docker deployment.
compatibility: Requires searxng-docker-tavily-adapter Docker stack running on localhost:8000
allowed-tools: Bash(curl *)
---

# SearXNG CLI (Tavily-Compatible)

Free, unlimited web search and content extraction via local Docker. No API key required.

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

## Workflow

| Need | Endpoint | When |
|------|----------|------|
| Find pages on a topic | `/search` | No specific URL yet |
| Get a page's content | `/search` + `include_raw_content` | Have a URL |

## Endpoints

- **Search**: `http://localhost:8000/search`
- **Health**: `http://localhost:8000/health`
- **SearXNG UI**: `http://localhost:8999`

## Quick start

```bash
curl -X POST "http://localhost:8000/search" \
  -H "Content-Type: application/json" \
  -d '{"query": "your query", "max_results": 5}'
```

## Python (drop-in Tavily replacement)

```python
from tavily import TavilyClient
client = TavilyClient(api_key="ignored", base_url="http://localhost:8000")
response = client.search(query="your query", max_results=5, include_raw_content=True)
```

## Advantages over Tavily

| Tavily | SearXNG Adapter |
|--------|-----------------|
| Paid | Free |
| API key required | No keys |
| Rate limits | No limits |
| External service | Local deployment |

## Limitations

- No AI-powered research synthesis → use tavily-research
- No advanced search filters (topic, time range) → use tavily-search
- No JavaScript rendering → use tavily-extract with `--extract-depth advanced`

## See also

- [searxng-search](../searxng-search/SKILL.md) — detailed search usage
- [searxng-extract](../searxng-extract/SKILL.md) — detailed extract usage
- [tavily-cli](../tavily-cli/SKILL.md) — original Tavily (requires API key)
