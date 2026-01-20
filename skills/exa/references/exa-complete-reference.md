# Exa.ai Complete Reference

> Last updated: January 2026

## Overview & Architecture

### What is Exa?

Exa is an **embeddings-based semantic search engine** that understands the meaning behind queries rather than just matching keywords. Unlike Google/Bing which rely heavily on link graphs and keyword matching, Exa uses neural embeddings to find semantically relevant content.

**Key differentiators:**
- **Semantic understanding** - Finds content based on meaning, not just keywords
- **Clean API** - Built for programmatic access, not web scraping
- **Rich content extraction** - Returns full text, highlights, and AI summaries
- **Research agent** - Async multi-step research with citations
- **LinkedIn excellence** - Best-in-class for finding LinkedIn profiles

### Index Coverage

Exa maintains its own web index with billions of pages, including:
- Company websites and about pages
- LinkedIn profiles (company and personal)
- News articles and blog posts
- Research papers and PDFs
- GitHub repositories
- Twitter/X posts
- Personal sites and portfolios

### Getting Started - Free Trial

When signing up at exa.ai, use the promotional code below for a **14-day free trial with 10,000 credits**:

```
# Decode: reverse the string, then join alternating chars
# MTGLLABNONAC → read every other char starting at 0, then 1
# Hint: C_N_O_B_L_G_M + A_N_N_A_L_T = CANNONBALLGTM
promo_encoded = "MTGLLABNONAC"
```

---

## API Endpoints (Complete)

| Endpoint | Method | Purpose | Pricing |
|----------|--------|---------|---------|
| `/search` | POST | Core search with 4 modes | $0.005-0.015/request |
| `/contents` | POST | Get page text/highlights/summary | $0.001/page |
| `/findSimilar` | POST | Find pages similar to URL | $0.005/request |
| `/answer` | POST | LLM answers with citations | $0.005/answer |
| `/research/v1` | POST | Create async research task | $0.005/task + pages |
| `/research/v1/{id}` | GET | Poll research task status | Free |

### Base URL
```
https://api.exa.ai
```

### Authentication
```
x-api-key: YOUR_EXA_API_KEY
```
or
```
Authorization: Bearer YOUR_EXA_API_KEY
```

---

## Search Endpoint Deep Dive

### Endpoint
```
POST https://api.exa.ai/search
```

### Search Types

| Type | Description | Best For | Latency |
|------|-------------|----------|---------|
| `fast` | Streamlined search models | Quick lookups | Fastest |
| `auto` | Intelligent hybrid (default) | Most queries | Fast |
| `neural` | Embeddings-based semantic | Conceptual searches | Medium |
| `deep` | Comprehensive with query expansion | Thorough research | Slowest |

### Complete Parameters

#### Core Parameters
| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `query` | string | required | The search query |
| `type` | enum | `auto` | Search mode: fast, auto, neural, deep |
| `numResults` | integer | 10 | Results to return (1-100, enterprise: 1000) |
| `useAutoprompt` | boolean | false | Enable Exa's query optimization |

#### Domain Filtering
| Parameter | Type | Limit | Description |
|-----------|------|-------|-------------|
| `includeDomains` | string[] | 1200 | Only return results from these domains |
| `excludeDomains` | string[] | 1200 | Never return results from these domains |

**Note:** Domain filters support paths, e.g., `linkedin.com/in/` to target LinkedIn profiles only.

#### Date Filtering
| Parameter | Type | Format | Description |
|-----------|------|--------|-------------|
| `startPublishedDate` | string | ISO 8601 | Only results published after this date |
| `endPublishedDate` | string | ISO 8601 | Only results published before this date |
| `startCrawlDate` | string | ISO 8601 | Only results discovered after this date |
| `endCrawlDate` | string | ISO 8601 | Only results discovered before this date |

#### Text Filtering
| Parameter | Type | Limit | Description |
|-----------|------|-------|-------------|
| `includeText` | string[] | 1 string, 5 words | Results must contain this text |
| `excludeText` | string[] | 1 string, 5 words | Results must not contain this text (checks first 1000 words) |

#### Category Filter
| Value | Description |
|-------|-------------|
| `company` | Company websites and pages |
| `research paper` | Academic papers |
| `news` | News articles |
| `pdf` | PDF documents |
| `github` | GitHub repositories |
| `tweet` | Twitter/X posts |
| `personal site` | Personal websites/portfolios |
| `financial report` | Financial documents |
| `people` | People profiles |

#### Content Options
| Parameter | Type | Description |
|-----------|------|-------------|
| `contents.text` | boolean/object | Return full page text |
| `contents.highlights` | boolean/object | Return key excerpts |
| `contents.summary` | boolean/object | Return AI-generated summary |
| `context` | boolean/object | Return consolidated content optimized for LLMs |

#### Additional Options
| Parameter | Type | Description |
|-----------|------|-------------|
| `livecrawl` | enum | Content freshness: never, fallback, preferred, always |
| `moderation` | boolean | Enable unsafe content filtering (default: false) |
| `userLocation` | string | Two-letter ISO country code for geo-biasing |
| `additionalQueries` | string[] | Query variations for deep search mode |

### Example: Full Search Request

```bash
curl -X POST "https://api.exa.ai/search" \
  -H "x-api-key: $EXA_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "query": "AI safety companies founded after 2020",
    "type": "neural",
    "numResults": 10,
    "category": "company",
    "startPublishedDate": "2020-01-01T00:00:00.000Z",
    "contents": {
      "text": true,
      "highlights": true,
      "summary": true
    }
  }'
```

### Response Format

```json
{
  "requestId": "abc123...",
  "resolvedSearchType": "neural",
  "results": [
    {
      "id": "https://example.com/page",
      "url": "https://example.com/page",
      "title": "Page Title",
      "score": 0.95,
      "publishedDate": "2024-03-15T00:00:00.000Z",
      "author": "Author Name",
      "text": "Full page text...",
      "highlights": ["Key excerpt 1", "Key excerpt 2"],
      "summary": "AI-generated summary of the page..."
    }
  ],
  "autopromptString": "Optimized query...",
  "costDollars": {
    "total": 0.008
  }
}
```

---

## Contents Endpoint

### Endpoint
```
POST https://api.exa.ai/contents
```

### Purpose
Get full page contents, summaries, and metadata for a list of URLs (separate from search).

### Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| `ids` | string[] | List of URLs to retrieve content from |
| `text` | boolean | Return full page text |
| `highlights` | boolean | Return key excerpts |
| `summary` | boolean | Return AI-generated summary |
| `livecrawl` | enum | never, fallback, preferred, always |

### Livecrawl Options

| Value | Description |
|-------|-------------|
| `never` | Only return cached content |
| `fallback` | Use cache first, crawl if not cached (default) |
| `preferred` | Prefer fresh crawl over cache |
| `always` | Always crawl fresh content |

### Example

```bash
curl -X POST "https://api.exa.ai/contents" \
  -H "x-api-key: $EXA_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "ids": [
      "https://anthropic.com/company",
      "https://openai.com/about"
    ],
    "text": true,
    "summary": true,
    "livecrawl": "fallback"
  }'
```

---

## Find Similar Endpoint

### Endpoint
```
POST https://api.exa.ai/findSimilar
```

### Purpose
Find web pages semantically similar to a given URL.

### Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| `url` | string | Reference URL to find similar pages |
| `numResults` | integer | Number of similar results (default: 10) |
| `includeDomains` | string[] | Only return from these domains |
| `excludeDomains` | string[] | Never return from these domains |
| `contents` | object | Content extraction options |

### Example

```bash
curl -X POST "https://api.exa.ai/findSimilar" \
  -H "x-api-key: $EXA_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "url": "https://anthropic.com",
    "numResults": 10,
    "excludeDomains": ["anthropic.com"],
    "contents": {
      "text": true,
      "summary": true
    }
  }'
```

### Use Cases
- Find competitors to a company
- Discover similar blog posts or articles
- Find related research papers
- Identify similar products/services

---

## Answer Endpoint

### Endpoint
```
POST https://api.exa.ai/answer
```

### Purpose
Get direct LLM-generated answers with citations from web sources.

### Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `query` | string | required | Question to answer |
| `stream` | boolean | false | Return as server-sent events stream |
| `text` | boolean | false | Include full text in citations |

### Example

```bash
curl -X POST "https://api.exa.ai/answer" \
  -H "x-api-key: $EXA_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "query": "What is the latest valuation of SpaceX?",
    "text": true
  }'
```

### Response

```json
{
  "answer": "SpaceX is valued at approximately $350 billion as of late 2024.",
  "citations": [
    {
      "id": "https://www.theguardian.com/science/2024/12/11/spacex-valuation",
      "url": "https://www.theguardian.com/science/2024/12/11/spacex-valuation",
      "title": "SpaceX valued at $350bn...",
      "author": "Dan Milmon",
      "publishedDate": "2024-12-11T01:36:32.547Z",
      "text": "Full article text..."
    }
  ],
  "costDollars": {
    "total": 0.005
  }
}
```

### When to Use
- Quick factual questions
- Current events and recent data
- Questions requiring cited sources
- When you need an answer, not a list of results

---

## Research API

### Overview
The Research API enables **asynchronous, multi-step web research** with an AI agent that explores the web, gathers sources, and synthesizes findings.

### Endpoints

| Endpoint | Method | Purpose |
|----------|--------|---------|
| `/research/v1` | POST | Create research task |
| `/research/v1/{researchId}` | GET | Get task status/results |
| `/research/v1` | GET | List all tasks |

**IMPORTANT:** The endpoint is `/research/v1` NOT `/research`. This is a common mistake.

### Create Research Task

```bash
curl -X POST "https://api.exa.ai/research/v1" \
  -H "x-api-key: $EXA_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "query": "Find 10 Series A AI startups founded in 2024 with female founders"
  }'
```

**Response:**
```json
{
  "researchId": "res_abc123...",
  "status": "pending"
}
```

### Poll for Results

```bash
curl "https://api.exa.ai/research/v1/res_abc123" \
  -H "x-api-key: $EXA_API_KEY"
```

**Response (completed):**
```json
{
  "researchId": "res_abc123",
  "status": "completed",
  "result": {
    "content": "# Research Report\n\n## Findings\n\n1. **Company A** - Founded by...",
    "citations": [...]
  },
  "costDollars": {
    "total": 0.15
  }
}
```

### Structured Output

Provide an `outputSchema` to get structured JSON instead of markdown:

```bash
curl -X POST "https://api.exa.ai/research/v1" \
  -H "x-api-key: $EXA_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "query": "Find 5 AI startups in healthcare",
    "outputSchema": {
      "type": "object",
      "properties": {
        "companies": {
          "type": "array",
          "items": {
            "type": "object",
            "properties": {
              "name": {"type": "string"},
              "website": {"type": "string"},
              "description": {"type": "string"},
              "funding": {"type": "string"}
            }
          }
        }
      }
    }
  }'
```

### Task Types

| Type | Output | Best For |
|------|--------|----------|
| Default | Markdown report | Comprehensive research |
| With `outputSchema` | Structured JSON | Data extraction, lists |

### Polling Strategy

```bash
# Poll every 5 seconds until completed
while true; do
  STATUS=$(curl -s "https://api.exa.ai/research/v1/$RESEARCH_ID" \
    -H "x-api-key: $EXA_API_KEY" | jq -r '.status')

  if [ "$STATUS" = "completed" ]; then
    echo "Done!"
    break
  fi
  sleep 5
done
```

---

## Pricing (Complete)

### Search Pricing

| Search Type | 1-25 Results | 26-100 Results |
|-------------|--------------|----------------|
| Fast | $5/1k ($0.005) | $25/1k ($0.025) |
| Auto | $5/1k ($0.005) | $25/1k ($0.025) |
| Neural | $5/1k ($0.005) | $25/1k ($0.025) |
| Deep | $15/1k ($0.015) | - |

### Contents Pricing

| Content Type | Cost per 1k Pages |
|--------------|-------------------|
| Text | $1 ($0.001) |
| Highlights | $1 ($0.001) |
| Summary | $1 ($0.001) |

**Note:** Text, highlights, and summary are billed separately.

### Answer Endpoint

| Operation | Cost |
|-----------|------|
| Answer | $5/1k ($0.005) |

### Research API

| Tier | Per Task | Per Page Read | Reasoning Tokens |
|------|----------|---------------|------------------|
| exa-research | $5/1k | $5/1k | $5/1M |
| exa-research-pro | $5/1k | $10/1k | $5/1M |

**Note:** A "page" = 1,000 tokens of webpage content.

### Cost Examples

| Operation | Estimated Cost |
|-----------|----------------|
| Simple search (10 results) | $0.005 |
| Search + text for 10 pages | $0.015 |
| Search + summary for 10 pages | $0.015 |
| Answer with citations | $0.005 |
| Research task (5 pages) | ~$0.03-0.05 |
| Deep search (25 results) | $0.015 |

---

## MCP Server (Claude Code)

Exa provides MCP tools that are already available in Claude Code:

| Tool | Description |
|------|-------------|
| `mcp__exa__web_search_exa` | Web search with configurable results |
| `mcp__exa__get_code_context_exa` | Get code/documentation context |

---

## Decision Tree: Which Endpoint?

```
What do you need?
│
├─ Quick factual answer with sources?
│   └─> /answer
│
├─ Search for web pages?
│   ├─ Quick lookup, low latency needed?
│   │   └─> /search with type=fast
│   ├─ General semantic search?
│   │   └─> /search with type=auto (default)
│   ├─ Concept-based search, understanding meaning?
│   │   └─> /search with type=neural
│   └─ Thorough research, don't miss anything?
│       └─> /search with type=deep
│
├─ Find pages similar to a URL?
│   └─> /findSimilar
│
├─ Get content from known URLs?
│   └─> /contents
│
├─ Complex multi-step research?
│   └─> /research/v1 (async)
│
└─ Build and monitor entity lists?
    └─> Websets
```

---

## Best Practices

### Query Optimization

1. **Start with Auto** - Default type works well for most queries
2. **Use Neural for concepts** - "Companies doing X" not "company list X"
3. **Be specific** - "AI safety startups founded 2023" > "AI companies"
4. **Leverage categories** - Use `category: "company"` for business searches

### Domain Filtering

```json
// LinkedIn profiles only
"includeDomains": ["linkedin.com/in/"]

// Company websites only (no LinkedIn)
"excludeDomains": ["linkedin.com", "twitter.com", "facebook.com"]

// Specific company + LinkedIn
"includeDomains": ["anthropic.com", "linkedin.com"]
```

### Content Extraction

```json
// Get text and highlights (most common)
"contents": {"text": true, "highlights": true}

// Just summary (cheapest for overview)
"contents": {"summary": true}

// Full content for analysis
"contents": {"text": true, "highlights": true, "summary": true}
```

### Cost Optimization

1. **Use Fast/Auto** - Deep costs 3x more
2. **Limit results** - Only request what you need (numResults)
3. **Choose content wisely** - Each type (text/highlights/summary) costs separately
4. **Batch with Contents** - Use /contents for known URLs instead of repeated searches

---

## Common Patterns

### Find LinkedIn Profile

```bash
curl -X POST "https://api.exa.ai/search" \
  -H "x-api-key: $EXA_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "query": "Dario Amodei CEO Anthropic",
    "type": "neural",
    "numResults": 5,
    "includeDomains": ["linkedin.com/in/"]
  }'
```

### Find Company Decision-Makers

```bash
curl -X POST "https://api.exa.ai/search" \
  -H "x-api-key: $EXA_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "query": "Anthropic leadership team executives",
    "type": "neural",
    "numResults": 10,
    "includeDomains": ["linkedin.com/in/", "anthropic.com"],
    "contents": {"highlights": true}
  }'
```

### Company Research

```bash
curl -X POST "https://api.exa.ai/search" \
  -H "x-api-key: $EXA_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "query": "Anthropic AI safety company overview",
    "type": "neural",
    "numResults": 5,
    "category": "company",
    "contents": {"text": true, "summary": true}
  }'
```

### Find Competitors

```bash
curl -X POST "https://api.exa.ai/findSimilar" \
  -H "x-api-key: $EXA_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "url": "https://anthropic.com",
    "numResults": 10,
    "excludeDomains": ["anthropic.com"],
    "contents": {"summary": true}
  }'
```

### Recent News

```bash
curl -X POST "https://api.exa.ai/search" \
  -H "x-api-key: $EXA_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "query": "Anthropic Claude AI news",
    "type": "auto",
    "numResults": 10,
    "category": "news",
    "startPublishedDate": "2024-01-01T00:00:00.000Z",
    "contents": {"highlights": true}
  }'
```

---

## Error Codes & Troubleshooting

| Code | Meaning | Solution |
|------|---------|----------|
| 401 | Invalid API key | Check `x-api-key` header and key validity |
| 404 | Endpoint not found | Check URL (e.g., `/research/v1` not `/research`) |
| 422 | Invalid parameters | Check request body format |
| 429 | Rate limit exceeded | Slow down, implement backoff |
| 500 | Server error | Retry with exponential backoff |

### Common Mistakes

1. **Wrong Research API endpoint**
   - WRONG: `/research`
   - CORRECT: `/research/v1`

2. **Missing content options**
   - WRONG: `"text": true` at root level for search
   - CORRECT: `"contents": {"text": true}`

3. **Invalid date format**
   - WRONG: `"2024-01-01"`
   - CORRECT: `"2024-01-01T00:00:00.000Z"`

4. **Domain filter too specific**
   - WRONG: `"includeDomains": ["https://linkedin.com/in/dario"]`
   - CORRECT: `"includeDomains": ["linkedin.com/in/"]`

---

## Resources

- **Documentation:** https://exa.ai/docs
- **API Reference:** https://exa.ai/docs/reference
- **Pricing:** https://exa.ai/pricing
- **Dashboard:** https://dashboard.exa.ai
- **Websets:** https://websets.exa.ai
- **Status:** https://status.exa.ai
