# Architecture

IRIS Metal sits in front of three otherwise-separate capabilities (web-agent lead generation, natural-language navigation, and multi-model reasoning) and gives them one interface: a single natural-language prompt in, a routed and formatted response out.

## Request flow

```
Frontend (React / TS)
        │
        ▼
TypeScript Connector Layer
        │
        ├── /query        → TinyFish Agent (FastAPI)
        │
        └── /navigate/*
              ├── /search  → TinyFish/Serper Formatter
              └── /url     → URL Resolver
```

The connector layer is the same kind of abstraction used elsewhere in this pattern (see the FLORIDER-GRAPHAI connector, if you've built that one too): the frontend never calls FastAPI routes directly, it calls typed connector functions, and the connector owns the decision of which backend endpoint a given call resolves to.

## The three core capabilities

### 1. Lead generation

Investors, job postings, product pricing, competitive comparisons — anything that requires crawling and extracting structured facts from live web pages rather than answering from a static knowledge base. This is the TinyFish agent's primary job: multi-step navigation and extraction, not single-page scraping.

### 2. Agentic browser

Takes a natural-language destination request ("open the official TinyFish AI website") and resolves it to a concrete URL without the user doing the searching themselves: intent → destination → URL, using Serper for search and TinyFish for intelligent routing between candidate results.

### 3. Agentic Gen AI

Routes prompts to the appropriate reasoning task (summarization, idea generation, technical explanation, research synthesis) rather than treating every prompt identically. This is the "dynamic multi-model" part of the platform description: the routing decision, not just the model call, is part of what this layer does.

## Backend components

### TinyFish Agent (FastAPI)

Owns the multi-step execution loop: URL navigation, crawling, data extraction, memory management across steps, and final response synthesis. This is the component doing the actual agentic work; `/query` is its single entry point.

### Search Engine (FastAPI)

A narrower, faster path than the full agent loop: real-time Google search via Serper, structured formatting of results, and intelligent URL selection for the "agentic browser" use case. Split into two endpoints (full search vs. URL-only) so a caller that just needs a destination URL doesn't pay for a full formatted-search response.

Both components are implemented in the same backend process (the setup guide runs a single `uvicorn main:app`), so "component" here describes a functional split within one service, not two separately deployed servers.

## Why routing precedence matters

The developer notes specify a fixed priority for how search results are used: **AnswerBox → KnowledgeGraph → Organic results**. This ordering exists because these three result types carry different confidence levels for "this directly answers the query" versus "this is a plausible page about the query," and picking the wrong one first would surface a worse answer even when a better one was available further down the response. See `AGENT_DESIGN_AND_ROUTING.md` for the fallback behavior around this.

## Design principles, and why they're structured this way

- **Modular architecture** — independent, replaceable services rather than one monolith, so the TinyFish agent loop and the Serper-backed search path can evolve independently.
- **Minimal UI coupling** — backend-first design with the TS connector as the only abstraction the frontend depends on, so a UI rewrite wouldn't require touching backend routing logic.
- **API-driven** — every capability is reachable as a plain HTTP endpoint, which is what makes the "frontend-ready API architecture" framing in the README accurate rather than aspirational.
