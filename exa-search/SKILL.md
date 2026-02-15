---
name: exa-search
description: Exa AI search API via curl. Use this skill to perform neural web search, find trending content, search tweets, blogs, companies, and retrieve full page contents.
vm0_secrets:
  - EXA_API_KEY
---

# Exa Search API

Use the Exa AI search API via direct `curl` calls to **perform semantic web search, find trending content, search tweets, and retrieve page contents**.

> Official docs: `https://exa.ai/docs`

---

## When to Use

Use this skill when you need to:

- **Search the web** with neural/semantic search (finds meaning, not just keywords)
- **Find trending content** across Reddit, Twitter/X, blogs, and news sites
- **Search tweets** from Twitter/X with date filtering
- **Find personal blogs and sites** on specific topics
- **Get full page contents** including text, summaries, and highlights
- **Research companies or people** across the web

---

## Prerequisites

1. Create an account at https://exa.ai
2. Get your API key from https://dashboard.exa.ai/api-keys

```bash
export EXA_API_KEY="your-api-key-here"
```

---

> **Important:** When using `$VAR` in a command that pipes to another command, wrap the command containing `$VAR` in `bash -c '...'`. Due to a Claude Code bug, environment variables are silently cleared when pipes are used directly.
> ```bash
> bash -c 'curl -s "https://api.exa.ai/search" -H "x-api-key: $EXA_API_KEY"'
> ```

## How to Use

Base URL: `https://api.exa.ai`

Authentication: `x-api-key` header

---

### 1. Basic Web Search

Search the web using Exa's neural search engine.

Write to `/tmp/exa_request.json`:

```json
{
  "query": "best new programming languages 2025",
  "numResults": 10,
  "type": "auto"
}
```

Then run:

```bash
bash -c 'curl -s -X POST "https://api.exa.ai/search" -H "x-api-key: ${EXA_API_KEY}" -H "Content-Type: application/json" -d @/tmp/exa_request.json'
```

---

### 2. Search and Get Contents (Combined)

Search and retrieve full page text in one call. This is the most useful endpoint.

Write to `/tmp/exa_request.json`:

```json
{
  "query": "viral programming topics this week",
  "numResults": 10,
  "type": "auto",
  "contents": {
    "text": {
      "maxCharacters": 3000
    },
    "highlights": {
      "numSentences": 3,
      "highlightsPerUrl": 3
    },
    "summary": {
      "query": "What is the main topic and why is it trending?"
    }
  }
}
```

Then run:

```bash
bash -c 'curl -s -X POST "https://api.exa.ai/search" -H "x-api-key: ${EXA_API_KEY}" -H "Content-Type: application/json" -d @/tmp/exa_request.json'
```

---

### 3. Search Tweets (Twitter/X)

Search Twitter/X content using the `tweet` category.

**Important restrictions for tweet category:**
- Do NOT use `includeText` / `excludeText` (causes 400 errors)
- Do NOT use `includeDomains` / `excludeDomains` (causes 400 errors)
- Use `livecrawl: "preferred"` for recent tweets

Write to `/tmp/exa_request.json`:

```json
{
  "query": "viral developer tool launch",
  "category": "tweet",
  "numResults": 20,
  "type": "auto",
  "startPublishedDate": "2025-01-01T00:00:00.000Z",
  "livecrawl": "preferred",
  "contents": {
    "text": {
      "maxCharacters": 1000
    }
  }
}
```

Then run:

```bash
bash -c 'curl -s -X POST "https://api.exa.ai/search" -H "x-api-key: ${EXA_API_KEY}" -H "Content-Type: application/json" -d @/tmp/exa_request.json'
```

---

### 4. Search Personal Blogs & Sites

Find individual perspectives and technical blogs.

Write to `/tmp/exa_request.json`:

```json
{
  "query": "lessons learned building production AI applications",
  "category": "personal site",
  "numResults": 15,
  "type": "deep",
  "startPublishedDate": "2025-01-01T00:00:00.000Z",
  "contents": {
    "summary": {
      "query": "What are the key lessons?"
    },
    "highlights": {
      "numSentences": 3,
      "highlightsPerUrl": 2
    }
  }
}
```

Then run:

```bash
bash -c 'curl -s -X POST "https://api.exa.ai/search" -H "x-api-key: ${EXA_API_KEY}" -H "Content-Type: application/json" -d @/tmp/exa_request.json'
```

---

### 5. Search with Domain Filtering

Restrict or exclude specific domains (NOT available for tweet category).

Write to `/tmp/exa_request.json`:

```json
{
  "query": "trending developer topics",
  "numResults": 15,
  "type": "auto",
  "includeDomains": ["reddit.com", "news.ycombinator.com", "dev.to"],
  "startPublishedDate": "2025-06-01T00:00:00.000Z",
  "contents": {
    "text": {
      "maxCharacters": 2000
    }
  }
}
```

Then run:

```bash
bash -c 'curl -s -X POST "https://api.exa.ai/search" -H "x-api-key: ${EXA_API_KEY}" -H "Content-Type: application/json" -d @/tmp/exa_request.json'
```

**Exclude domains:**

```json
{
  "query": "best programming frameworks",
  "numResults": 10,
  "type": "auto",
  "excludeDomains": ["medium.com", "quora.com"]
}
```

---

### 6. Search with Date Filtering

Filter results by publication or crawl date. Dates use ISO 8601 format.

Write to `/tmp/exa_request.json`:

```json
{
  "query": "AI breakthroughs",
  "numResults": 10,
  "type": "auto",
  "startPublishedDate": "2025-06-01T00:00:00.000Z",
  "endPublishedDate": "2025-06-30T23:59:59.000Z",
  "contents": {
    "text": {
      "maxCharacters": 2000
    }
  }
}
```

Then run:

```bash
bash -c 'curl -s -X POST "https://api.exa.ai/search" -H "x-api-key: ${EXA_API_KEY}" -H "Content-Type: application/json" -d @/tmp/exa_request.json'
```

---

### 7. Get Contents for Known URLs

Retrieve full text, summaries, and highlights for specific URLs.

Write to `/tmp/exa_request.json`:

```json
{
  "ids": [
    "https://example.com/article-1",
    "https://example.com/article-2"
  ],
  "text": {
    "maxCharacters": 5000
  },
  "summary": {
    "query": "Summarize the main points"
  },
  "highlights": {
    "numSentences": 5,
    "highlightsPerUrl": 3
  }
}
```

Then run:

```bash
bash -c 'curl -s -X POST "https://api.exa.ai/contents" -H "x-api-key: ${EXA_API_KEY}" -H "Content-Type: application/json" -d @/tmp/exa_request.json'
```

---

### 8. Deep Search with Summaries

Use `type: "deep"` for comprehensive results with smart query expansion.

Write to `/tmp/exa_request.json`:

```json
{
  "query": "most discussed open source projects this week",
  "numResults": 10,
  "type": "deep",
  "livecrawl": "preferred",
  "contents": {
    "summary": {
      "query": "Why is this project getting attention? What makes it notable?"
    },
    "highlights": {
      "numSentences": 3,
      "highlightsPerUrl": 2
    }
  }
}
```

Then run:

```bash
bash -c 'curl -s -X POST "https://api.exa.ai/search" -H "x-api-key: ${EXA_API_KEY}" -H "Content-Type: application/json" -d @/tmp/exa_request.json'
```

---

### 9. Company Search

Find information about companies.

Write to `/tmp/exa_request.json`:

```json
{
  "query": "AI startups raising Series A 2025",
  "category": "company",
  "numResults": 10,
  "type": "auto",
  "contents": {
    "summary": {
      "query": "What does this company do and how much funding?"
    }
  }
}
```

Then run:

```bash
bash -c 'curl -s -X POST "https://api.exa.ai/search" -H "x-api-key: ${EXA_API_KEY}" -H "Content-Type: application/json" -d @/tmp/exa_request.json'
```

---

### 10. Fast/Instant Search

For low-latency results when speed matters most.

Write to `/tmp/exa_request.json`:

```json
{
  "query": "latest tech news today",
  "numResults": 5,
  "type": "instant"
}
```

Then run:

```bash
bash -c 'curl -s -X POST "https://api.exa.ai/search" -H "x-api-key: ${EXA_API_KEY}" -H "Content-Type: application/json" -d @/tmp/exa_request.json'
```

---

## Search Parameters Reference

| Parameter | Type | Description |
|-----------|------|-------------|
| `query` | string | **Required.** Search query |
| `numResults` | integer | Number of results to return (default: 10) |
| `type` | string | `"auto"`, `"neural"`, `"fast"`, `"instant"`, `"deep"` |
| `category` | string | `"tweet"`, `"personal site"`, `"company"`, `"people"` |
| `includeDomains` | string[] | Only include these domains (not for tweets) |
| `excludeDomains` | string[] | Exclude these domains (not for tweets) |
| `includeText` | string[] | Must contain this text — single item only (not for tweets) |
| `excludeText` | string[] | Exclude if contains — single item only (not for tweets) |
| `startPublishedDate` | string | ISO 8601 date filter (start) |
| `endPublishedDate` | string | ISO 8601 date filter (end) |
| `startCrawlDate` | string | Filter by crawl date (start) |
| `endCrawlDate` | string | Filter by crawl date (end) |
| `livecrawl` | string | `"preferred"`, `"always"`, `"fallback"`, `"never"` |
| `livecrawlTimeout` | integer | Timeout for livecrawl in ms |
| `userLocation` | string | ISO 3166-1 alpha-2 country code for geographic bias |

## Contents Options (nested under `contents`)

| Parameter | Type | Description |
|-----------|------|-------------|
| `text.maxCharacters` | integer | Max characters of page text to return |
| `highlights.numSentences` | integer | Sentences per highlight |
| `highlights.highlightsPerUrl` | integer | Number of highlights per result |
| `highlights.query` | string | Custom query for highlight relevance |
| `summary.query` | string | Custom query for AI-generated summary |

## Category Restrictions

| Category | Domain Filters | Text Filters | Livecrawl |
|----------|---------------|--------------|-----------|
| *(none)* | Supported | Supported | Supported |
| `tweet` | **NOT supported** | **NOT supported** | Supported |
| `personal site` | Supported | Single-item only | Supported |
| `company` | Supported | Supported | Supported |
| `people` | Supported | Supported | Supported |

---

## Response Format

```json
{
  "requestId": "uuid",
  "resolvedSearchType": "neural",
  "results": [
    {
      "title": "Article Title",
      "url": "https://example.com/article",
      "publishedDate": "2025-06-15T10:30:00.000Z",
      "author": "Author Name",
      "score": 0.95,
      "id": "https://example.com/article",
      "text": "Full page text content...",
      "highlights": ["Key sentence 1.", "Key sentence 2."],
      "summary": "AI-generated summary of the content."
    }
  ]
}
```

---

## Practical Examples

### Find Viral Reddit Discussions This Week

```json
{
  "query": "most upvoted programming discussions",
  "numResults": 15,
  "type": "deep",
  "includeDomains": ["reddit.com"],
  "startPublishedDate": "2025-06-08T00:00:00.000Z",
  "livecrawl": "preferred",
  "contents": {
    "text": { "maxCharacters": 2000 },
    "summary": { "query": "What is being discussed and why is it popular?" }
  }
}
```

### Find Trending Dev YouTube Videos

```json
{
  "query": "popular programming tutorial new framework",
  "numResults": 10,
  "type": "auto",
  "includeDomains": ["youtube.com"],
  "startPublishedDate": "2025-06-01T00:00:00.000Z",
  "contents": {
    "summary": { "query": "What is the video about and why is it trending?" }
  }
}
```

### Search Viral Tech Tweets

```json
{
  "query": "viral developer tool launch open source",
  "category": "tweet",
  "numResults": 20,
  "type": "auto",
  "startPublishedDate": "2025-06-08T00:00:00.000Z",
  "livecrawl": "preferred",
  "contents": {
    "text": { "maxCharacters": 1000 }
  }
}
```

---

## Guidelines

1. **Search type selection**: Use `auto` for general queries, `deep` for comprehensive research, `instant`/`fast` for speed
2. **Livecrawl**: Use `"preferred"` when freshness matters (tweets, news, trending topics)
3. **Category restrictions**: Do NOT use domain/text filters with `tweet` category
4. **Text filters**: `includeText` and `excludeText` accept only single-item arrays
5. **Date format**: Always use ISO 8601 format (`2025-01-15T00:00:00.000Z`)
6. **Contents extraction**: Include `contents` object in search to get text/summaries in one call instead of two
7. **Rate limits**: Check your plan at https://dashboard.exa.ai for rate limit details
