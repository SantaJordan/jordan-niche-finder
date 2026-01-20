# Exa Skill Execution

You are executing the Exa skill. Exa is an embeddings-based semantic search engine with multiple products.

## Input
- Query: {query}
- Mode: {mode} (optional: search type or endpoint)

## Decision Tree: Which Endpoint?

```
What does the user need?
│
├─ Quick factual answer with sources?
│   └─> /answer endpoint
│
├─ Search for web pages?
│   ├─ Quick lookup, low latency needed?
│   │   └─> /search type=fast
│   ├─ General semantic search?
│   │   └─> /search type=auto (default)
│   ├─ Concept-based search?
│   │   └─> /search type=neural
│   └─ Thorough research, comprehensive results?
│       └─> /search type=deep
│
├─ Find pages similar to a URL?
│   └─> /findSimilar endpoint
│
├─ Get content from known URLs?
│   └─> /contents endpoint
│
├─ Complex multi-step research?
│   └─> /research/v1 (async)
│
└─ Build and monitor entity lists?
    └─> Websets (dashboard or API)
```

---

## Endpoint 1: Search

**Use for:** Finding web pages, LinkedIn profiles, company research

```bash
curl -X POST "https://api.exa.ai/search" \
  -H "x-api-key: $EXA_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "query": "{query}",
    "type": "auto",
    "numResults": 10,
    "contents": {
      "text": true,
      "highlights": true
    }
  }'
```

### Search Types

| Type | Best For | Cost |
|------|----------|------|
| `fast` | Quick lookups, low latency | $0.005 |
| `auto` | Most queries (default) | $0.005 |
| `neural` | Semantic/concept searches | $0.005 |
| `deep` | Comprehensive research | $0.015 |

### Key Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| `query` | string | Search query (required) |
| `type` | enum | fast, auto, neural, deep |
| `numResults` | int | 1-100 (enterprise: 1000) |
| `includeDomains` | string[] | Only these domains |
| `excludeDomains` | string[] | Never these domains |
| `category` | enum | company, news, research paper, pdf, github, tweet, personal site, people |
| `startPublishedDate` | ISO 8601 | After this date |
| `endPublishedDate` | ISO 8601 | Before this date |
| `includeText` | string[] | Must contain (1 string, 5 words max) |
| `excludeText` | string[] | Must not contain |
| `contents.text` | bool | Return full page text |
| `contents.highlights` | bool | Return key excerpts |
| `contents.summary` | bool | Return AI summary |
| `livecrawl` | enum | never, fallback, preferred, always |
| `moderation` | bool | Filter unsafe content |

### Response

```json
{
  "results": [
    {
      "id": "https://example.com/page",
      "url": "https://example.com/page",
      "title": "Page Title",
      "score": 0.95,
      "publishedDate": "2024-03-15T00:00:00.000Z",
      "text": "Full page text...",
      "highlights": ["Key excerpt 1", "Key excerpt 2"],
      "summary": "AI-generated summary..."
    }
  ],
  "costDollars": {"total": 0.008}
}
```

---

## Endpoint 2: Find Similar

**Use for:** Finding competitors, related content, similar pages

```bash
curl -X POST "https://api.exa.ai/findSimilar" \
  -H "x-api-key: $EXA_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "url": "{reference_url}",
    "numResults": 10,
    "excludeDomains": ["{exclude_source_domain}"],
    "contents": {
      "summary": true
    }
  }'
```

### Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| `url` | string | Reference URL (required) |
| `numResults` | int | Number of results |
| `includeDomains` | string[] | Limit to these domains |
| `excludeDomains` | string[] | Exclude these domains |
| `contents` | object | Content extraction options |

---

## Endpoint 3: Contents

**Use for:** Getting page content from known URLs

```bash
curl -X POST "https://api.exa.ai/contents" \
  -H "x-api-key: $EXA_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "ids": ["{url_1}", "{url_2}"],
    "text": true,
    "summary": true,
    "livecrawl": "fallback"
  }'
```

### Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| `ids` | string[] | URLs to fetch (required) |
| `text` | bool | Return full text |
| `highlights` | bool | Return key excerpts |
| `summary` | bool | Return AI summary |
| `livecrawl` | enum | never, fallback, preferred, always |

### Livecrawl Options

| Value | Description |
|-------|-------------|
| `never` | Only cached content |
| `fallback` | Cache first, crawl if needed (default) |
| `preferred` | Prefer fresh crawl |
| `always` | Always crawl fresh |

---

## Endpoint 4: Answer

**Use for:** Direct answers with citations, factual questions

```bash
curl -X POST "https://api.exa.ai/answer" \
  -H "x-api-key: $EXA_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "query": "{question}",
    "text": true
  }'
```

### Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| `query` | string | Question to answer (required) |
| `text` | bool | Include full text in citations |
| `stream` | bool | Stream response via SSE |

### Response

```json
{
  "answer": "The answer to the question...",
  "citations": [
    {
      "id": "https://source.com/article",
      "url": "https://source.com/article",
      "title": "Source Title",
      "publishedDate": "2024-12-01T00:00:00.000Z",
      "text": "Full source text..."
    }
  ],
  "costDollars": {"total": 0.005}
}
```

---

## Endpoint 5: Research API (Async)

**Use for:** Complex multi-step research, list building, deep investigations

**IMPORTANT:** Endpoint is `/research/v1` NOT `/research`

### Step 1: Create Task

```bash
curl -X POST "https://api.exa.ai/research/v1" \
  -H "x-api-key: $EXA_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "query": "{research_query}"
  }'
```

**Response:**
```json
{
  "researchId": "res_abc123...",
  "status": "pending"
}
```

### Step 2: Poll for Results

```bash
curl "https://api.exa.ai/research/v1/{researchId}" \
  -H "x-api-key: $EXA_API_KEY"
```

**Response (completed):**
```json
{
  "researchId": "res_abc123",
  "status": "completed",
  "result": {
    "content": "# Research Report\n\n## Findings\n\n...",
    "citations": [...]
  },
  "costDollars": {"total": 0.15}
}
```

### Structured Output

For JSON output instead of markdown, provide `outputSchema`:

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
              "description": {"type": "string"}
            }
          }
        }
      }
    }
  }'
```

### Polling Strategy

Poll every 5-10 seconds until `status` is `completed` or `failed`.

---

## Common Use Cases

### Find LinkedIn Profile (Person)

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

### Find LinkedIn Profile (Company)

```bash
curl -X POST "https://api.exa.ai/search" \
  -H "x-api-key: $EXA_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "query": "Anthropic AI company",
    "type": "neural",
    "numResults": 5,
    "includeDomains": ["linkedin.com/company/"]
  }'
```

### Find Company Decision-Makers

```bash
curl -X POST "https://api.exa.ai/search" \
  -H "x-api-key: $EXA_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "query": "Anthropic leadership team executives founders",
    "type": "neural",
    "numResults": 10,
    "includeDomains": ["linkedin.com/in/"],
    "contents": {"highlights": true}
  }'
```

### Company Research

```bash
curl -X POST "https://api.exa.ai/search" \
  -H "x-api-key: $EXA_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "query": "Anthropic AI safety company overview about",
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
    "query": "Anthropic Claude AI",
    "type": "auto",
    "numResults": 10,
    "category": "news",
    "startPublishedDate": "2024-01-01T00:00:00.000Z",
    "contents": {"highlights": true}
  }'
```

### Quick Fact Answer

```bash
curl -X POST "https://api.exa.ai/answer" \
  -H "x-api-key: $EXA_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "query": "When was Anthropic founded and who are the founders?",
    "text": true
  }'
```

---

## MCP Tools Available

Claude Code has built-in Exa MCP tools:

### web_search_exa
```
mcp__exa__web_search_exa
- query: Search query
- numResults: Number of results (default: 8)
- type: auto, fast, deep
- livecrawl: fallback, preferred
- contextMaxCharacters: Max chars for context (default: 10000)
```

### get_code_context_exa
```
mcp__exa__get_code_context_exa
- query: Code/documentation search query
- tokensNum: Tokens to return (1000-50000, default: 5000)
```

---

## Pricing Quick Reference

| Operation | Cost |
|-----------|------|
| Search (1-25 results) | $0.005 |
| Search (26-100 results) | $0.025 |
| Deep search (1-25 results) | $0.015 |
| Text per page | $0.001 |
| Highlights per page | $0.001 |
| Summary per page | $0.001 |
| Answer | $0.005 |
| Research task | $0.005 |
| Research page read | $0.005-0.01 |

---

## Output Format

```
## Exa Results for {query}

### Endpoint Used: {endpoint}
### Search Type: {type}

### Results
1. **{title}** (Score: {score})
   - URL: {url}
   - Published: {publishedDate}
   - Highlights: {highlights[]}
   - Summary: {summary}

2. **{title}** (Score: {score})
   ...

### LinkedIn Profiles Found
- {linkedin_urls[]}

### Cost: ${total_cost}
```

---

## Error Handling

| Code | Meaning | Solution |
|------|---------|----------|
| 401 | Invalid API key | Check x-api-key header |
| 404 | Wrong endpoint | Use /research/v1 not /research |
| 422 | Invalid parameters | Check request body |
| 429 | Rate limit | Implement backoff |

### Common Mistakes

1. **Research API:** Use `/research/v1` NOT `/research`
2. **Contents in search:** Use `"contents": {"text": true}` not `"text": true`
3. **Date format:** Use ISO 8601 `"2024-01-01T00:00:00.000Z"`
4. **Domain paths:** `"linkedin.com/in/"` not `"https://linkedin.com/in/user"`

---

## Score Interpretation

| Score | Confidence |
|-------|------------|
| > 0.9 | Very high - exact match |
| 0.7-0.9 | Good - relevant result |
| 0.5-0.7 | Moderate - possible match |
| < 0.5 | Low - weak relevance |

---

## Reference

For complete documentation: `references/exa-complete-reference.md`
