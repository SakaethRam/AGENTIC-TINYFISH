# API Reference

## Backend endpoints (FastAPI, single service)

### `POST /query` — TinyFish Agent

The full agentic loop: multi-step navigation, crawling, extraction, and response synthesis.

**Request**

```json
{
  "query": "Find 5 investors in AI startups"
}
```

**Response**

```json
{
  "response": "Formatted output..."
}
```

Use this for anything that needs multi-step reasoning or web crawling: lead generation, comparative research, "find N things matching criteria X."

### `POST /tinyfish/search` — Full Search

Real-time Google search via Serper, returned as structured, formatted output. This is the heavier of the two search-engine endpoints; use it when the caller wants a formatted result set, not just a single URL.

### `POST /tinyfish/url` — URL Picker (Agent Mode)

Resolves a natural-language destination request directly to a single URL, applying the AnswerBox → KnowledgeGraph → Organic priority described in `ARCHITECTURE.md`. This is the endpoint behind the "agentic browser" capability ("open the official website for X").

## TypeScript connector functions

The connector layer is what the frontend actually calls; it wraps the endpoints above.

```ts
getTinyFishResponse(input: string): Promise<string>
```
Calls `/query`. Use for lead generation and general agentic queries.

```ts
getSerperResponse(query: string): Promise<string>
```
Calls `/tinyfish/search`. Use for formatted search results.

```ts
getTopSerperUrl(query: string): Promise<string>
```
Calls `/tinyfish/url`. Use for direct navigation ("open ...") requests.

```ts
getUnifiedResponse(input: string): Promise<string>
```
Optional unified router: a single entry point that internally decides which of the three functions above to call, so the frontend doesn't have to classify the user's intent itself. Whether this exists as a real implementation or a documented extension point depends on the current state of the connector source; confirm against the actual TypeScript file before depending on it.

## Choosing an endpoint

| Intent | Endpoint | Connector function |
|--------|----------|---------------------|
| "Find me N things matching criteria" | `/query` | `getTinyFishResponse` |
| "Search for X" (formatted results) | `/tinyfish/search` | `getSerperResponse` |
| "Open / take me to X" | `/tinyfish/url` | `getTopSerperUrl` |
| Unclassified natural-language input | any of the above, or `/query` as the general-purpose fallback | `getUnifiedResponse`, if implemented |

## Example prompts, by endpoint

**`/query` (lead generation, general agentic)**
- "Find 5 investors actively investing in voice AI startups with their portfolio companies"
- "Give me leads on 3 branded shoes around $500 with their product page"
- "Find 2 Software Engineer job openings listed on LinkedIn"

**`/tinyfish/url` (agentic browser)**
- "Open the official website for TinyFish AI"
- "Open the latest Tesla news from a reliable source"

**`/query` (agentic Gen AI / reasoning)**
- "Summarize the latest trends in AI agents with examples"
- "Explain RAG architecture in simple terms"
