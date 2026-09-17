# IRIS Metal: Agentic AI powered by TinyFish

A unified agentic intelligence platform that enables real-time lead generation, zero-friction web navigation, and dynamic multi-model AI responses through a single interface powered by TinyFish API.

### [Visit: IRIS • METAL ](https://metal-ai-saas.vercel.app)

<img width="1800" height="1000" alt="ZREX TINYFISH" src="https://github.com/user-attachments/assets/a105ddde-6ec3-4779-942a-d13625af6c45" />

---

## Platform Overview

IRIS Metal is designed as a modular agentic system that combines:

* Autonomous web agents (TinyFish-powered)
* Real-time search intelligence (Serper API)
* Multi-step reasoning and data extraction
* Structured output formatting via LLMs
* Frontend-ready API architecture

It allows users to interact with the internet, data sources, and AI models using simple natural language commands.

---

## Core Capabilities

### 1. Lead Generation

Generates actionable leads across:

* Investors
* Stock markets
* Job postings
* Product pricing insights
* Competitive comparisons

All powered by real-time crawling and extraction via TinyFish agents.

---

### 2. Agentic Browser

Transforms natural language into direct navigation:

* Resolves intent → finds destination → returns URL
* Eliminates manual searching
* Uses Serper + TinyFish for intelligent routing

---

### 3. Agentic Gen AI

Routes prompts dynamically to AI models:

* Summarization
* Idea generation
* Technical explanations
* Research synthesis

---

## Example Prompts

### Lead Generation

* Find 5 investors actively investing in voice AI startups with their portfolio companies
* Give me leads on 3 branded shoes around $500 with their product page
* Find 2 Software Engineer job openings listed on LinkedIn

### Agentic Browser

* Open the official website for TinyFish AI
* Open "Soda Pop" song
* Open the latest Tesla news from a reliable source

### Agentic Gen AI

* Summarize the latest trends in AI agents with examples
* Generate a startup idea using AI + Web3
* Explain RAG architecture in simple terms

---

## System Architecture

```
Frontend (React / TS)
        │
        ▼
TypeScript Connector Layer
        │
        ├── /query → TinyFish Agent (FastAPI)
        │
        └── /navigate/*
              ├── /search → TinyFish/serper Formatter
              └── /url    → URL Resolver
```

---

## Backend Components

### 1. TinyFish Agent (FastAPI)

Handles:

* Multi-step execution loop
* URL navigation + crawling
* Data extraction
* Memory management
* Response synthesis

#### Endpoint

```
POST /query
```

#### Request

```json
{
  "query": "Find 5 investors in AI startups"
}
```

#### Response

```json
{
  "response": "Formatted output..."
}
```

---

### 2. Search Engine (FastAPI)

Handles:

* Real-time Google search via Serper API
* Structured formatting
* Intelligent URL selection

#### Endpoints

##### Full Search

```
POST /tinyfish/search
```

##### URL Picker (Agent Mode)

```
POST /tinyfish/url
```

---

## TypeScript Connector

Acts as the bridge between frontend and backend.

### Functions

#### TinyFish Agent

```ts
getTinyFishResponse(input: string): Promise<string>
```

#### Serper Search

```ts
getSerperResponse(query: string): Promise<string>
```

#### URL Resolver

```ts
getTopSerperUrl(query: string): Promise<string>
```

#### Unified Router (Optional)

```ts
getUnifiedResponse(input: string): Promise<string>
```

---

## Key Design Principles

### Agentic Execution

* Multi-step reasoning
* URL discovery and recursion
* Context-aware memory

### Real-Time Intelligence

* Live search via Serper
* Dynamic data extraction

### Modular Architecture

* Independent services
* Replaceable components
* Scalable design

### Minimal UI Coupling

* Backend-first design
* TS connector abstraction
* API-driven system

---

## Developer Notes

* The system prioritizes:

  * AnswerBox → KnowledgeGraph → Organic results
* Supports:

  * Direct URL extraction
  * Natural language navigation ("open ...")
* Includes:

  * Fallback mechanisms for failures
  * Structured formatting for readability

---

## Future Enhancements

* Streaming responses (SSE → frontend)
* Advanced ranking/scoring of extracted data
* Multi-agent orchestration
* Local vector memory (RAG)
* Plugin-based tool execution

---

## License & Contribution

AGENTIC TINYFISH is distributed under the terms defined in `LICENSE`.
