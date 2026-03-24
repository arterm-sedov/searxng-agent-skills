# SearXNG Agent Skills

Agent skills for [searxng-docker-tavily-adapter](https://github.com/vakovalskii/searxng-docker-tavily-adapter) - free, unlimited web search for AI agents.

## Skills

| Skill | Description |
|-------|-------------|
| `searxng-cli` | Overview, workflow, endpoints |
| `searxng-search` | Web search with LLM-optimized results |
| `searxng-extract` | URL content extraction |

## Why SearXNG Skills Exist

Web search is essential for AI agents, but most solutions require paid APIs with rate limits. The SearXNG skills exist to provide a free, unlimited, local alternative that works seamlessly with AI agent workflows.

- **Free** — No API keys, no subscriptions, no limits
- **Local** — Runs on your machine via Docker
- **Unlimited** — No rate limits or quotas
- **Drop-in replacement** — Compatible with Tavily client

These skills provide specialized interfaces for different search tasks: general search, content extraction, and CLI overview.

## Prerequisites

Docker stack from [searxng-docker-tavily-adapter](https://github.com/vakovalskii/searxng-docker-tavily-adapter):

```bash
# 1. Clone and configure
git clone https://github.com/vakovalskii/searxng-docker-tavily-adapter
cd searxng-docker-tavily-adapter
cp config.example.yaml config.yaml

# 2. Generate secret key and update config
python -c "import secrets; print(secrets.token_urlsafe(32))"
# Edit config.yaml: replace secret_key value with generated key

# 3. Start Docker stack
docker compose up -d

# 4. Verify
curl http://localhost:8000/health
```

**Endpoints:**
- Search API: `http://localhost:8000/search`
- SearXNG UI: `http://localhost:8999`

## Installation

Copy skills to your agent's skills directory:

```bash
cp -r skills/* ~/.agents/skills/
```

## Quick Start

```bash
curl -X POST "http://localhost:8000/search" \
  -H "Content-Type: application/json" \
  -d '{"query": "your query", "max_results": 5}'
```

## Python (Drop-in Tavily Replacement)

```python
from tavily import TavilyClient
client = TavilyClient(api_key="ignored", base_url="http://localhost:8000")
response = client.search(query="your query", max_results=5)
```

## Advantages over Tavily

| Tavily | SearXNG |
|--------|---------|
| Paid | Free |
| API key required | No keys |
| Rate limits | No limits |
| External service | Local |

## Related

- [searxng-docker-tavily-adapter](https://github.com/vakovalskii/searxng-docker-tavily-adapter) - Docker stack
