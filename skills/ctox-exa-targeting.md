---
name: ctox-exa-targeting
description: Build Exa.ai queries to find perfect-fit customers based on the CTO's validated niche
---

# CTOx Exa Targeting Skill

## When to Use

Use this skill AFTER:
1. Niche is triangulated (Industry × Situation × Unique Angle)
2. Insights are validated (Counterintuitive, Specific, Quantifiable, Demonstrable)

**DO NOT USE** until both phases are complete.

## Exa.ai Overview

Exa is an embeddings-based semantic search engine. It finds content by MEANING, not just keywords.

### Key Capabilities

| Endpoint | Use For | Cost |
|----------|---------|------|
| `/search` | Finding companies, people, news | $0.005-0.015 |
| `/findSimilar` | Finding lookalike companies | $0.005 |
| `/contents` | Getting page content | $0.001/page |
| `/answer` | Quick factual lookups | $0.005 |
| `/research/v1` | Complex multi-step research (async) | $0.005+ |

## The Column Strategy

For every niche, build 3-5 data columns that identify perfect-fit customers:

```
┌─────────────────────────────────────────────────────────────┐
│                   TARGETING COLUMNS                         │
│                                                             │
│   ┌──────────────┐  ┌──────────────┐  ┌──────────────┐     │
│   │   TRIGGER    │  │   INDUSTRY   │  │    PAIN      │     │
│   │    EVENT     │  │    MATCH     │  │  INDICATOR   │     │
│   │              │  │              │  │              │     │
│   │ Companies in │  │ Right        │  │ Evidence of  │     │
│   │ situation    │  │ vertical     │  │ the problem  │     │
│   │ NOW          │  │              │  │              │     │
│   └──────────────┘  └──────────────┘  └──────────────┘     │
│                                                             │
│   ┌──────────────┐  ┌──────────────┐                       │
│   │    TECH      │  │  DECISION    │                       │
│   │   SIGNAL     │  │   MAKER      │                       │
│   │              │  │              │                       │
│   │ Technology   │  │ Who to       │                       │
│   │ relevance    │  │ contact      │                       │
│   └──────────────┘  └──────────────┘                       │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

## Column Types & Exa Queries

### Column 1: Trigger Event

**Purpose:** Find companies in the specific SITUATION right now.

**Exa Query Pattern:**
```json
{
  "query": "[trigger event] [industry] companies announced",
  "type": "neural",
  "numResults": 25,
  "category": "news",
  "startPublishedDate": "2024-10-01T00:00:00.000Z",
  "contents": {
    "highlights": true,
    "summary": true
  }
}
```

**Examples by Situation:**

| Situation | Query |
|-----------|-------|
| Post-acquisition | `"healthcare SaaS company acquired" OR "healthcare tech acquisition announced"` |
| Post-Series A | `"series A funding healthcare SaaS"` |
| CTO departure | `"CTO leaving" OR "CTO departed" site:linkedin.com` |
| Failed audit | `"SOC 2 audit failed" OR "compliance issues"` |
| Tech migration | `"legacy system migration" OR "cloud migration announcement"` |

### Column 2: Industry Match

**Purpose:** Find companies in the right vertical.

**Exa Query Pattern:**
```json
{
  "query": "[industry] [sub-vertical] company",
  "type": "neural",
  "numResults": 50,
  "category": "company",
  "contents": {
    "summary": true
  }
}
```

**Or use findSimilar from a known customer:**
```json
{
  "url": "https://known-customer.com",
  "numResults": 25,
  "excludeDomains": ["known-customer.com"],
  "contents": {
    "summary": true
  }
}
```

### Column 3: Pain Indicator

**Purpose:** Find evidence companies have the PROBLEM.

**Job Postings (pain signal):**
```json
{
  "query": "[pain keyword] engineer hiring [industry]",
  "type": "neural",
  "numResults": 25,
  "includeDomains": ["linkedin.com/jobs/", "greenhouse.io", "lever.co", "ashbyhq.com"],
  "contents": {
    "text": true
  }
}
```

**News/Blog Mentions:**
```json
{
  "query": "[problem keyword] challenges [industry]",
  "type": "neural",
  "numResults": 25,
  "category": "news",
  "startPublishedDate": "2024-07-01T00:00:00.000Z",
  "contents": {
    "highlights": true
  }
}
```

**Examples by Pain:**

| Pain | Query |
|------|-------|
| Integration debt | `"integration challenges" OR "data silos" [industry]` |
| Tech debt | `"technical debt" OR "legacy modernization" [industry]` |
| Scaling issues | `"scaling challenges" OR "performance issues" [industry]` |
| Compliance | `"compliance requirements" OR "audit preparation" [industry]` |

### Column 4: Tech Signal

**Purpose:** Find companies with relevant technology context.

**Stack Detection:**
```json
{
  "query": "[technology] [framework] [industry] company",
  "type": "neural",
  "numResults": 25,
  "contents": {
    "text": true
  }
}
```

**Job Posting Analysis:**
```json
{
  "query": "[tech keyword] engineer [company name]",
  "type": "neural",
  "numResults": 10,
  "includeDomains": ["linkedin.com/jobs/"],
  "contents": {
    "text": true
  }
}
```

### Column 5: Decision Maker

**Purpose:** Find the person to contact.

**LinkedIn Search:**
```json
{
  "query": "CTO [company name] [industry]",
  "type": "neural",
  "numResults": 5,
  "includeDomains": ["linkedin.com/in/"],
  "contents": {
    "highlights": true
  }
}
```

**VP Engineering/Tech Lead:**
```json
{
  "query": "VP Engineering OR Head of Engineering [company name]",
  "type": "neural",
  "numResults": 5,
  "includeDomains": ["linkedin.com/in/"],
  "contents": {
    "highlights": true
  }
}
```

## Full Example: Healthcare M&A CTO

**Niche:** "I help healthcare SaaS companies in the first 90 days post-acquisition merge their tech stacks using parallel-run migration."

### Column Strategy

| Column | Purpose | Exa Query |
|--------|---------|-----------|
| `recent_acquisition` | Find recently acquired companies | See below |
| `healthcare_saas` | Verify healthcare SaaS vertical | See below |
| `integration_pain` | Evidence of integration challenges | See below |
| `tech_stack_signal` | Technology context | See below |
| `cto_contact` | Who to reach | See below |

### Query: Recent Acquisition
```json
{
  "query": "healthcare SaaS company acquired OR healthcare technology acquisition announced",
  "type": "neural",
  "numResults": 30,
  "category": "news",
  "startPublishedDate": "2024-07-01T00:00:00.000Z",
  "contents": {
    "highlights": true,
    "summary": true
  }
}
```

### Query: Healthcare SaaS Verification
```json
{
  "query": "healthcare SaaS platform EHR integration",
  "type": "neural",
  "numResults": 50,
  "category": "company",
  "contents": {
    "summary": true
  }
}
```

### Query: Integration Pain
```json
{
  "query": "integration engineer healthcare hiring OR data integration healthcare SaaS",
  "type": "neural",
  "numResults": 25,
  "includeDomains": ["linkedin.com/jobs/", "greenhouse.io", "lever.co"],
  "contents": {
    "text": true
  }
}
```

### Query: Tech Stack Signal
```json
{
  "query": "[company name] engineering blog OR [company name] tech stack",
  "type": "neural",
  "numResults": 10,
  "contents": {
    "text": true,
    "summary": true
  }
}
```

### Query: CTO Contact
```json
{
  "query": "CTO [company name] healthcare",
  "type": "neural",
  "numResults": 5,
  "includeDomains": ["linkedin.com/in/"],
  "contents": {
    "highlights": true
  }
}
```

## Research API for Complex Queries

For complex, multi-step research, use the async Research API:

**IMPORTANT:** Endpoint is `/research/v1` NOT `/research`

### Step 1: Create Task
```bash
curl -X POST "https://api.exa.ai/research/v1" \
  -H "x-api-key: $EXA_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "query": "Find healthcare SaaS companies acquired in the last 6 months that are now hiring integration engineers"
  }'
```

### Step 2: Poll for Results
```bash
curl "https://api.exa.ai/research/v1/{researchId}" \
  -H "x-api-key: $EXA_API_KEY"
```

### Structured Output (JSON)
```json
{
  "query": "Find 10 healthcare SaaS companies acquired in 2024",
  "outputSchema": {
    "type": "object",
    "properties": {
      "companies": {
        "type": "array",
        "items": {
          "type": "object",
          "properties": {
            "name": {"type": "string"},
            "acquirer": {"type": "string"},
            "date": {"type": "string"},
            "website": {"type": "string"}
          }
        }
      }
    }
  }
}
```

## Output Format

Deliver the targeting strategy as:

```markdown
## Targeting Columns for [CTO Name]

### Niche: [One sentence]

### Column 1: [Name]
**Purpose:** [What it finds]
**Signal:** [What this indicates]
**Exa Query:**
\`\`\`json
{query JSON}
\`\`\`

### Column 2: [Name]
**Purpose:** [What it finds]
**Signal:** [What this indicates]
**Exa Query:**
\`\`\`json
{query JSON}
\`\`\`

[Repeat for all columns]

### Sample Companies Found
1. [Company A] - [Why they match]
2. [Company B] - [Why they match]
3. [Company C] - [Why they match]

### Sample Outreach Message

> "[Pain-qualified message that is a redescription of the targeting]"
```

## The Message Is The Targeting

Remember: "The message is a redescription of the targeting."

Once you have the columns, the outreach message writes itself:

**Example:**
- **Targeting:** Healthcare SaaS acquired in last 6 months + hiring integration engineers
- **Message:** "Saw [Company] was acquired by [Acquirer] in [Month]. You're now hiring integration engineers - that's usually the first sign that merging two tech stacks is getting painful. I help healthcare SaaS companies get through the first 90 days post-acquisition using parallel-run migration..."

## Pricing Reference

| Operation | Cost |
|-----------|------|
| Search (1-25 results) | $0.005 |
| Search (26-100 results) | $0.025 |
| Deep search | $0.015 |
| Content per page | $0.001 |
| Research task | $0.005 + pages |

Typical targeting build: $0.05-0.15 total
