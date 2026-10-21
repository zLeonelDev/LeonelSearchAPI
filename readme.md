# LeonelSearchAPI

Official JavaScript client for the **LeonelSearchAPI**.  
Easily perform web searches and retrieve structured results using your API key.

---

## Features

- 🔍 **Multi-type search support**: `web`, `image`, `video`, and `news`
- ⚡ **Lightweight**: No dependencies, fast and efficient
- 📦 **Modern ES Modules**: Built with `import/export` syntax
- 📑 **Structured results**: Consistent data format across all search types
- 🔐 **Secure authentication**: API key protected requests
- 🌐 **Simple integration**: Easy to use in any JavaScript project
- 🎯 **Convenience methods**: Dedicated methods for each search type

---

## Installation

```bash
npm install leonelsearchapi
```

---

## Quick Start

```javascript
import { LeonelSearchAPI } from "leonelsearchapi";

// Initialize the client
const client = new LeonelSearchAPI({
  apiKey: "YOUR_API_KEY", // Get your key from leonelsearch.qzz.io
  debug: false, // Set true to enable debug logs
});

// Perform a web search
async function searchExample() {
  try {
    const results = await client.search({
      query: "latest space news",
      type: "web", // Search type: 'web', 'image', 'video', or 'news'
      limit: 5,
      page: 1,
    });

    console.log(results);
  } catch (err) {
    console.error("Error:", err.message);
  }
}

searchExample();
```

---

## API Reference

### `new LeonelSearchAPI({ apiKey })`

- **apiKey** _(string, required)_: Your personal LeonelSearchAPI key

### `client.search({ query, type, limit, page })`

The main search method supporting all search types.

| Parameter | Type   | Required | Default | Description                | Valid Values                            |
| --------- | ------ | -------- | ------- | -------------------------- | --------------------------------------- |
| query     | string | ✅       | —       | The search term            | Any string                              |
| type      | string | ✅       | —       | Type of search to perform  | `'web'`, `'image'`, `'video'`, `'news'` |
| limit     | number | ❌       | 10      | Number of results per page | 1-50                                    |
| page      | number | ❌       | 1       | Page number                | ≥1                                      |

**Returns:** A Promise resolving to a structured response object:

```json
{
  "query": "search query",
  "results": [
    {
      "type": "result type",
      "url": "source URL",
      "title": "result title",
      "text": "description or excerpt",
      "score": 0
    }
  ]
}
```

### Convenience Methods

For easier use, the client provides dedicated methods for each search type:

#### `client.searchWeb(query, limit = 10, page = 1)`

Performs a web search.

#### `client.searchImages(query, limit = 10, page = 1)`

Performs an image search.

#### `client.searchVideos(query, limit = 10, page = 1)`

Performs a video search.

#### `client.searchNews(query, limit = 10, page = 1)`

Performs a news search.

---

## Examples

### Using the Main Search Method

```javascript
// Web search
const webResults = await client.search({
  query: "latest space exploration",
  type: "web",
  limit: 5,
});

// Image search
const imageResults = await client.search({
  query: "mars rover photos",
  type: "image",
  limit: 10,
});

// Video search
const videoResults = await client.search({
  query: "quantum computing explained",
  type: "video",
  limit: 3,
});

// News search
const newsResults = await client.search({
  query: "artificial intelligence breakthroughs",
  type: "news",
  limit: 5,
  page: 2,
});
```

### Using Convenience Methods

```javascript
// More concise syntax for common use cases
const webResults = await client.searchWeb("space exploration", 10, 1);
const imageResults = await client.searchImages("natural landscapes", 15, 1);
const videoResults = await client.searchVideos("cooking tutorials", 5, 1);
const newsResults = await client.searchNews("technology trends", 8, 1);
```

### Complete Example with Error Handling

```javascript
import { LeonelSearchAPI } from "leonelsearchapi";

const client = new LeonelSearchAPI({
  apiKey: "your-api-key-here",
});

async function performComprehensiveSearch() {
  try {
    console.log("=== Performing Multiple Search Types ===\n");

    // Search for web results
    const webSearch = await client.searchWeb("renewable energy innovations", 3);
    console.log("Web Results:", webSearch.results.length, "items found");

    // Search for images
    const imageSearch = await client.searchImages(
      "solar panels installation",
      4,
    );
    console.log("Image Results:", imageSearch.results.length, "items found");

    // Search for news
    const newsSearch = await client.searchNews("climate policy updates", 5);
    console.log("News Results:", newsSearch.results.length, "items found\n");

    // Display sample results
    console.log("Sample News Headlines:");
    newsSearch.results.slice(0, 3).forEach((result, index) => {
      console.log(`${index + 1}. ${result.title}`);
    });

    return { webSearch, imageSearch, newsSearch };
  } catch (error) {
    console.error("Search failed:", error.message);
    throw error;
  }
}

performComprehensiveSearch();
```

### Pagination Example

```javascript
async function fetchPaginatedResults() {
  const allResults = [];
  let currentPage = 1;
  const resultsPerPage = 10;

  try {
    // Fetch first page
    let response = await client.searchNews(
      "economic indicators",
      resultsPerPage,
      currentPage,
    );
    allResults.push(...response.results);

    // Continue fetching while there are results
    while (response.results.length === resultsPerPage) {
      currentPage++;
      response = await client.searchNews(
        "economic indicators",
        resultsPerPage,
        currentPage,
      );
      allResults.push(...response.results);
    }

    console.log(`Total results fetched: ${allResults.length}`);
    return allResults;
  } catch (error) {
    console.error("Pagination error:", error.message);
    return allResults;
  }
}
```

---

## Response Structure

All search methods return results in a consistent format:

```javascript
{
  query: "original search query",
  results: [
    {
      type: "web", // or "image", "video", "news"
      url: "https://example.com/article",
      title: "Article Title",
      text: "Article excerpt or description...",
      score: 0 // Relevance score
    },
    // ... more results
  ]
}
```

---

## Error Handling

```javascript
import { LeonelSearchAPI } from "leonelsearchapi";

const client = new LeonelSearchAPI({ apiKey: "your-key" });

async function safeSearch(query, type = "web") {
  try {
    const results = await client.search({ query, type });
    return results;
  } catch (error) {
    // Handle specific error types
    const errorMessage = error.message.toLowerCase();

    if (errorMessage.includes("401") || errorMessage.includes("invalid api")) {
      console.error("❌ Authentication failed. Please check your API key.");
    } else if (
      errorMessage.includes("429") ||
      errorMessage.includes("rate limit")
    ) {
      console.error(
        "⏳ Rate limit exceeded. Please wait before making more requests.",
      );
    } else if (
      errorMessage.includes("400") ||
      errorMessage.includes("bad request")
    ) {
      console.error(
        "⚠️  Invalid request. Please check your search parameters.",
      );
    } else if (
      errorMessage.includes("network") ||
      errorMessage.includes("connection")
    ) {
      console.error("🔌 Network error. Please check your internet connection.");
    } else {
      console.error("💥 An unexpected error occurred:", error.message);
    }

    // Return empty results for graceful degradation
    return { query, results: [] };
  }
}

// Usage
const results = await safeSearch("test query", "news");
```

---

## TypeScript Support

The package includes full TypeScript definitions:

```typescript
import { LeonelSearchAPI, SearchResult, SearchParams } from "leonelsearchapi";

const client = new LeonelSearchAPI({ apiKey: "your-key" });

// Type-safe search with main method
async function searchWithTypes(params: SearchParams) {
  const response = await client.search(params);
  return response.results;
}

// Using convenience methods with type inference
async function convenienceExamples() {
  // These methods are fully typed
  const webResults = await client.searchWeb("typescript tutorial", 5);
  const imageResults = await client.searchImages("typescript logo", 10);
  const newsResults = await client.searchNews("typescript updates", 8);

  return { webResults, imageResults, newsResults };
}

// Type-safe parameter object
const searchParams: SearchParams = {
  query: "typescript vs javascript",
  type: "web", // TypeScript will validate this value
  limit: 5,
  page: 1,
};
```

---

## Best Practices

### 1. **API Key Security**

```javascript
// Store API key in environment variables, not in code
const client = new LeonelSearchAPI({
  apiKey: process.env.LEONELSEARCH_API_KEY,
});
```

### 2. **Request Optimization**

```javascript
// Request only what you need
const optimalSearch = await client.search({
  query: "specific topic",
  type: "web",
  limit: 5, // Smaller limits for faster responses
});
```

### 3. **Implement Caching**

```javascript
// Simple caching strategy
const searchCache = new Map();

async function cachedSearch(query, type = "web") {
  const cacheKey = `${type}:${query}`;

  if (searchCache.has(cacheKey)) {
    return searchCache.get(cacheKey);
  }

  const results = await client.search({ query, type });
  searchCache.set(cacheKey, results);

  // Optional: Clear cache after some time
  setTimeout(() => searchCache.delete(cacheKey), 300000); // 5 minutes

  return results;
}
```

### 4. **Rate Limiting**

```javascript
// Implement request spacing
async function rateLimitedSearch(query, type) {
  await new Promise((resolve) => setTimeout(resolve, 1000)); // 1 second delay
  return client.search({ query, type });
}
```

### 5. **Batch Processing**

```javascript
// Process multiple searches efficiently
async function batchSearch(queries, type = "web") {
  const results = [];

  for (const query of queries) {
    try {
      const result = await client.search({ query, type, limit: 3 });
      results.push({ query, success: true, data: result });
    } catch (error) {
      results.push({ query, success: false, error: error.message });
    }

    // Delay between requests to respect rate limits
    await new Promise((resolve) => setTimeout(resolve, 500));
  }

  return results;
}
```

---

## Publishing

The package is published to npm automatically by GitHub Actions.

### One-time setup

1. Create an **Automation access token** on npmjs.com
   (Profile → Access Tokens → Generate New Token → *Automation*).
2. In the GitHub repo go to **Settings → Secrets and variables → Actions** and add:
   - Name: `NPM_TOKEN`
   - Value: the token you just created.

### Releasing a new version

```bash
npm version patch   # or minor / major — updates package.json + package-lock.json
git push origin main --follow-tags
gh release create v1.0.3 --title "v1.0.3" --generate-notes
```

Publishing a **GitHub Release** triggers `.github/workflows/publish.yml`, which:

1. Runs the `verify` job (install + smoke test + `npm pack --dry-run`).
2. Runs `npm publish` in the `publish` job using `secrets.NPM_TOKEN`.

You can also trigger it manually from the **Actions** tab with `workflow_dispatch`
(there is a `dry-run` input that only prints what would be published).

---

## License

MIT © 2026 LeonelDev

---

## Support

For API key registration, documentation, and support, visit [leonelsearchapi.qzz.io](https://leonelsearchapi.qzz.io)

---

_Note: This is a beta release. API responses and features may change during development._
