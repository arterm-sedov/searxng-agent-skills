---
name: searxng-extract
description: |
  Use when the user has URLs and wants their content, says "extract", "grab the content from", "pull the text from", "get the page at", "read this webpage" — AND mentions "free", "no API key", "local", "private", or wants a free alternative to Tavily extract. Extracts clean text via local Docker deployment.
compatibility: Requires Docker stack at D:/Repo/searxng-docker-tavily-adapter (run `docker compose up -d`)
allowed-tools: Bash(curl *)
---

# searxng extract

Extract clean text from URLs via local SearXNG. No API key required.

## Prerequisites Check

```bash
curl -s http://localhost:8000/health || echo "Docker stack not running"
```

If down: `cd D:/Repo/searxng-docker-tavily-adapter && docker compose up -d`

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

Edit `D:/Repo/searxng-docker-tavily-adapter/config.yaml`:

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
