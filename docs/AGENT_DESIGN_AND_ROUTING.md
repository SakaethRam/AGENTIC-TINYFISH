# Agent Design and Routing

## Result precedence: AnswerBox → KnowledgeGraph → Organic

When the search engine component resolves a query, it does not treat all Serper result types as equally trustworthy. The stated precedence is:

1. **AnswerBox** — a direct, already-synthesized answer Serper itself extracted (e.g. a featured snippet). Highest priority because it's the closest thing to "already answered."
2. **KnowledgeGraph** — structured entity data (e.g. official site, canonical facts about a company or person). Used when there's no direct answer box but a clear canonical entity match exists.
3. **Organic results** — the standard ranked list of web pages. Fallback when neither of the above is available; this is where "reasonable URL that's probably right" lives.

This ordering is what makes `/tinyfish/url` behave like a human doing a quick search and clicking the obviously-correct top result, rather than always taking the first organic link regardless of whether a better-quality signal was available.

## Agentic execution loop (TinyFish Agent, `/query`)

- **Multi-step reasoning** — a single query can require several navigation/extraction steps rather than one page fetch; the agent loop is what sequences those steps.
- **URL discovery and recursion** — the agent can follow links it discovers mid-task (e.g. from a search results page to individual investor profile pages) rather than being limited to a single fixed URL per query.
- **Context-aware memory** — state persists across the steps of a single multi-step task, so step 3 can reference what step 1 extracted.

## Fallback mechanisms

The README notes fallback mechanisms exist for failures, without specifying exact behavior per failure type. Practically, a system built around this precedence should degrade gracefully at each level:

- No AnswerBox → fall through to KnowledgeGraph.
- No KnowledgeGraph match → fall through to organic results.
- No usable organic result → return a clear "couldn't resolve" response rather than an empty or malformed one, so the frontend has something deterministic to render.

Confirm the actual fallback implementation in the backend source before relying on this as documented behavior; treat it here as the intended design the precedence rule implies.

## Structured formatting for readability

Both `/query` and `/tinyfish/search` responses are meant to be formatted for readability, not raw API dumps. This matters for the frontend: it can generally render `response` fields directly rather than needing to do its own post-processing of search-engine output.

## Natural language navigation ("open ...")

The `/tinyfish/url` path is specifically built to recognize direct-navigation phrasing ("open X", "take me to X") and treat those as a single-URL resolution task rather than a general search task. This is a deliberate intent split from `/tinyfish/search`, which returns a result set rather than committing to one destination.

## Where this is headed (from the README's stated roadmap)

- Streaming responses (SSE to frontend), which would change `/query` from a single request/response into an incremental stream — worth designing the connector's function signatures with this in mind if you're extending `getTinyFishResponse` later.
- Advanced ranking/scoring of extracted data, likely refining the AnswerBox/KnowledgeGraph/Organic precedence into something more graduated.
- Multi-agent orchestration and local vector memory (RAG), both of which would extend the "context-aware memory" property above beyond a single request's lifetime.
- Plugin-based tool execution, which would generalize the current fixed TinyFish/Serper toolset into something extensible.
