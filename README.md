# SearXNG Agent Skills

Free, unlimited web search skills for AI agents - a local alternative to Tavily.

## Skills

| Skill | Description |
|-------|-------------|
| `searxng-cli` | Overview, workflow, endpoints |
| `searxng-search` | Web search with LLM-optimized results |
| `searxng-extract` | URL content extraction |

## Installation

Copy skills to your agent's skills directory:

```bash
cp -r skills/* ~/.agents/skills/
```

## Prerequisites

Docker stack running at `D:/Repos/searxng-docker-tavily-adapter`:

```bash
cd D:/Repos/searxng-docker-tavily-adapter
docker compose up -d
curl http://localhost:8000/health  # verify
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

## Documentation

- [TDD Experiment](docs/TDD-EXPERIMENT.md) - How these skills were perfected

## Related

- [searxng-docker-tavily-adapter](https://github.com/vakovalskii/searxng-docker-tavily-adapter) - Docker stack
