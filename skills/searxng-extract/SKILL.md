---
name: searxng-extract
description: |
  Use ONLY when the user explicitly wants a FREE, LOCAL URL extraction solution — says "free extract", "no API key extract", "local extract", "self-hosted extract", "SearXNG extract", "free alternative to Tavily extract", or mentions wanting extraction without API keys or paid services. Do NOT use for general URL extraction — use tavily-extract instead.
compatibility: Requires searxng-docker-tavily-adapter Docker stack running on localhost:8000
allowed-tools: Bash(curl *)
---

# searxng extract

Extract clean text from URLs via local SearXNG. No API key required.

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

- Have specific URLs and want their content
- User mentions "free", "no API key", "local"
- Privacy-focused (local deployment)

## When NOT to use

- Need JavaScript-rendered pages → use tavily-extract with `--extract-depth advanced`
- Need query-focused chunking → use tavily-extract with `--query`
- Docker unavailable → use tavily-extract

## Quick start

```bash
curl -X POST "http://localhost:8000/search" \
  -H "Content-Type: application/json" \
  -d '{"query": "site:example.com", "max_results": 1, "include_raw_content": true}'
```

Note: SearXNG extracts via search + scraping. For direct URL, search for the specific URL.

## Response

```json
{
  "results": [{
    "url": "https://example.com/page",
    "title": "Page Title",
    "raw_content": "Full page text up to 2500 characters..."
  }]
}
```

## Configuration

Edit `config.yaml` in your adapter directory:

```yaml
adapter:
  scraper:
    timeout: 10              # Seconds per page
    max_content_length: 2500 # Max chars per page
```

## Tips

- **Content truncated to 2500 chars** per page (configurable)
- **Timeout is 10 seconds** per page (configurable)
- **No API key needed** — completely free

## See also

- [searxng-search](../searxng-search/SKILL.md) — search the web
- [tavily-extract](../tavily-extract/SKILL.md) — original Tavily extract
