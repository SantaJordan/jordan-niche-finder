---
name: exa
description: Comprehensive Exa.ai skill - semantic search, Research API, Answer endpoint, Websets, LinkedIn discovery, and company research
---

# Exa API Expert Skill

Use this skill when working with Exa.ai - the embeddings-based semantic search engine. This skill covers **all** Exa products and APIs.

## When to Invoke This Skill

- Semantic web search (meaning-based, not just keywords)
- Finding LinkedIn profiles (company or person)
- Company research and competitor analysis
- Answering factual questions with citations
- Multi-step research tasks (Research API)
- Building and monitoring entity lists (Websets)
- Finding similar pages to a reference URL
- Getting current information from the web

## Complete API Coverage

| Endpoint | Purpose | Cost |
|----------|---------|------|
| `/search` | Core search (4 modes: fast/auto/neural/deep) | $0.005-0.015 |
| `/contents` | Get page text/highlights/summary | $0.001/page |
| `/findSimilar` | Find pages similar to URL | $0.005 |
| `/answer` | LLM answers with citations | $0.005 |
| `/research/v1` | Async agentic research | $0.005/task + pages |

## Products Covered

### Search API
- **4 search modes:** fast, auto (default), neural, deep
- **Rich filtering:** domains, dates, categories, text inclusion/exclusion
- **Content extraction:** text, highlights, AI summaries
- **Best for:** LinkedIn discovery, company research, semantic queries

### Answer Endpoint
- Direct LLM-generated answers with source citations
- Streaming support for real-time responses
- **Best for:** Factual questions, current events, quick lookups

### Research API
- Async multi-step research with AI agent
- Structured JSON output via `outputSchema`
- Markdown reports with citations (default)
- **Best for:** Complex research, list building, deep dives

### Websets
- Automated list building and monitoring
- Dashboard (no-code) and API access
- Enrichments, imports, webhooks
- **Best for:** Sales leads, recruiting, market research

## Quick Pricing Reference

| Operation | Cost |
|-----------|------|
| Search (1-25 results, fast/auto/neural) | $0.005 |
| Search (26-100 results) | $0.025 |
| Deep search (1-25 results) | $0.015 |
| Content text per page | $0.001 |
| Content highlights per page | $0.001 |
| Content summary per page | $0.001 |
| Answer | $0.005 |
| Research task | $0.005 + $0.005-0.01/page |

**Typical company research:** $0.01-0.02

## MCP Tools Available

Claude Code has built-in Exa MCP tools:

| Tool | Purpose |
|------|---------|
| `mcp__exa__web_search_exa` | Web search with configurable results |
| `mcp__exa__get_code_context_exa` | Get code/documentation context |

## Key Strengths

- **LinkedIn discovery** - Best-in-class for finding profiles
- **Semantic understanding** - Finds content by meaning, not keywords
- **Research automation** - Async Research API for complex queries
- **Clean API** - Built for programmatic access
- **Content extraction** - Full text, highlights, and AI summaries

## Example Use Cases

```
# Find a person's LinkedIn
/exa "John Smith CEO TechCorp" linkedin

# Research a company
/exa "Anthropic AI safety company overview"

# Find competitors
/exa similar:https://anthropic.com

# Quick factual answer
/exa answer:"When was OpenAI founded?"

# Deep research (async)
/exa research:"Find 10 AI startups in healthcare Series A 2024"
```

## Reference Documentation

For complete API documentation, parameters, and code examples:

**`references/exa-complete-reference.md`**

Covers:
- All endpoint parameters
- Complete pricing matrix
- Search mode comparison
- Websets documentation
- Integration guides (Python, JS, LangChain, etc.)
- Decision tree for choosing endpoints
- Best practices and common patterns
- Error codes and troubleshooting
