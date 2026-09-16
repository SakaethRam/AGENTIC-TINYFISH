# Contributing

## Before you start

- If your change touches `TINYFISH_API_KEY`, `GEMINI_API_KEY`, or `API_KEY` handling, prefer moving further toward environment-variable configuration over adding new hardcoded values; see `SETUP_AND_CONFIGURATION.md` for the current state and recommended fix.
- Changes to the AnswerBox → KnowledgeGraph → Organic precedence in the search engine component should be called out explicitly in the PR, since it changes what `/tinyfish/url` resolves to for the same query.
- Changes to the TinyFish agent's multi-step loop (navigation, recursion depth, memory handling) should include a before/after example query so reviewers can see the behavioral difference, not just the diff.

## Workflow

1. Fork the repository.
2. Create a feature branch.
3. Implement your change, scoped to one component: TinyFish Agent, Search Engine, TS Connector, or frontend.
4. Test against a running local backend (`uvicorn main:app --reload`) with real or sandbox API keys before opening a PR.
5. Submit a pull request with a clear description of what changed and, for routing/precedence changes, a couple of example queries showing the new behavior.

## Where to make changes

| Area | Location |
|------|----------|
| Agentic execution loop | `back-end/` (TinyFish Agent, `/query`) |
| Search/formatting/URL resolution | `back-end/` (Search Engine, `/tinyfish/search`, `/tinyfish/url`) |
| Connector functions | `src/` (TypeScript connector) |
| Frontend | `src/` (React/TS UI) |
| CI | `.github/workflows/` |

## Roadmap items open for contribution

From the README's stated future enhancements, in case you're looking for a starting point:

- Streaming responses (SSE) for `/query`
- Advanced ranking/scoring of extracted data
- Multi-agent orchestration
- Local vector memory (RAG)
- Plugin-based tool execution

## Reporting issues

Use the repository's Issues tab for bugs, unexpected routing behavior, or documentation gaps. If you find that a real API key was ever committed to history (see the hardcoded-key note in `SETUP_AND_CONFIGURATION.md`), treat that as a security matter: rotate the affected key immediately rather than just removing it from a future commit, since git history retains it regardless.
