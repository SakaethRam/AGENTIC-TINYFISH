# Setup and Configuration

## Clone and backend setup

```bash
git clone <repo-url>
cd agentic-tinyfish

pip install fastapi uvicorn httpx
uvicorn main:app --reload
```

Server runs at `http://localhost:8000`.

## Frontend setup

```bash
const BASE_API = "http://localhost:8000";
```

Point the connector's base URL constant at wherever the backend is actually running (local, staging, or production) before building the frontend.

## Environment variables — important note

The source repository's setup instructions currently say to **replace these directly in code**:

```
TINYFISH_API_KEY
GEMINI_API_KEY
API_KEY
```

This means, as documented, these three keys are hardcoded into the Python source rather than read from environment variables or a `.env` file. That is worth flagging explicitly rather than quietly working around:

- A key hardcoded in source is a key that ends up in git history the moment it's committed, and stays there even if a later commit removes it.
- It also means the same source file can't be deployed to two environments (e.g. local test vs. production) with different keys without editing code between deployments.

**Recommended fix, before deploying this anywhere beyond a local machine:**

```python
import os
TINYFISH_API_KEY = os.environ["TINYFISH_API_KEY"]
GEMINI_API_KEY = os.environ["GEMINI_API_KEY"]
API_KEY = os.environ["API_KEY"]
```

paired with a `.env` file (excluded via `.gitignore`) for local development and real environment variables in whatever platform hosts the deployed service. The Dockerfile and CI workflow in this delivery both assume this fix is in place — they pass these three values in as container environment variables rather than baking them into an image, which only works correctly once the source itself reads from `os.environ` instead of a hardcoded literal.

## Full local setup checklist

- [ ] `pip install fastapi uvicorn httpx` (or `pip install -r requirements.txt` if one exists)
- [ ] Move `TINYFISH_API_KEY`, `GEMINI_API_KEY`, `API_KEY` out of source and into environment variables (see above)
- [ ] `uvicorn main:app --reload` — confirm it's reachable at `http://localhost:8000`
- [ ] Set the frontend's `BASE_API` constant to match
- [ ] Run a sample query against `/query` to confirm the TinyFish and Gemini keys are valid before testing the full frontend flow
