# LeonelSearchAPI

JavaScript client for the LeonelSearchAPI. It lets you run web, image, video, and news searches from your projects using an API key.

## Installation

```bash
npm install leonelsearchapi
```

## Usage

```javascript
import { LeonelSearchAPI } from "leonelsearchapi";

const client = new LeonelSearchAPI({
  apiKey: process.env.LEONELSEARCH_API_KEY,
});

const response = await client.search({
  query: "latest space news",
  type: "web",
  limit: 5,
});

console.log(response.results);
```

Store your API key in an environment variable instead of hardcoding it in your source code.

## Search Types

The `search()` method supports the following types:

| Type | Description |
| --- | --- |
| `web` | Web pages and search results |
| `image` | Images |
| `video` | Videos |
| `news` | News articles |

## API Reference

### `client.search(options)`

Runs a search. It accepts an object with the following properties:

| Parameter | Type | Required | Default | Description |
| --- | --- | --- | --- | --- |
| `query` | `string` | Yes | — | Search query |
| `type` | `string` | Yes | — | Search type: `web`, `image`, `video`, or `news` |
| `limit` | `number` | No | `10` | Number of results per page (1–50) |
| `page` | `number` | No | `1` | Page number (starting at 1) |

### Search-specific methods

You can also use a dedicated method for each search type:

```javascript
const web = await client.searchWeb("space exploration", 10, 1);
const images = await client.searchImages("natural landscapes", 15, 1);
const videos = await client.searchVideos("cooking tutorials", 5, 1);
const news = await client.searchNews("technology trends", 8, 1);
```

Each method accepts `query`, `limit`, and `page`, in that order.

## Response Structure

The methods return a promise that resolves to an object containing the original query and its results. Each result has the following structure:

```javascript
{
  query: "search query",
  results: [
    {
      type: "web",
      url: "https://example.com/article",
      title: "Article title",
      text: "Result description or excerpt",
      score: 0
    }
  ]
}
```

The `type` value depends on the requested search type.

## TypeScript Support

The package includes TypeScript definitions. You can import `SearchParams` and `SearchResult` alongside the client:

```typescript
import {
  LeonelSearchAPI,
  type SearchParams,
  type SearchResult,
} from "leonelsearchapi";

const client = new LeonelSearchAPI({
  apiKey: process.env.LEONELSEARCH_API_KEY!,
});

async function search(params: SearchParams): Promise<SearchResult[]> {
  const response = await client.search(params);
  return response.results;
}
```

## Error Handling

Requests can fail if the API key is invalid, a rate limit is reached, or the request parameters are incorrect. Use `try/catch` to handle these cases:

```javascript
try {
  const response = await client.search({
    query: "technology",
    type: "news",
  });

  console.log(response.results);
} catch (error) {
  console.error("Search failed:", error.message);
}
```

## License

MIT © 2026 LeonelDev

## Links

- Official site: [LeonelSearchAPI](https://leonelsearch.qzz.io)
