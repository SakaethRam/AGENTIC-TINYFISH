# IRIS Metal — backend (FastAPI)
# Single service exposing /query, /tinyfish/search, /tinyfish/url.
#
# IMPORTANT: as documented upstream, the source currently expects
# TINYFISH_API_KEY / GEMINI_API_KEY / API_KEY to be hardcoded in
# main.py rather than read via os.environ. The ENV lines below only
# take effect once that's fixed — see docs/SETUP_AND_CONFIGURATION.md.
# Until then, this image will run, but the three keys still need to be
# edited into source before building, not supplied at `docker run`.

FROM python:3.11-slim

WORKDIR /app

COPY requirements.txt* ./
RUN pip install --no-cache-dir fastapi uvicorn httpx \
    && if [ -f requirements.txt ]; then pip install --no-cache-dir -r requirements.txt; fi

COPY . .

ENV TINYFISH_API_KEY=""
ENV GEMINI_API_KEY=""
ENV API_KEY=""
ENV PORT=8000

EXPOSE 8000

HEALTHCHECK --interval=30s --timeout=5s --start-period=10s --retries=3 \
    CMD python -c "import urllib.request; urllib.request.urlopen('http://localhost:8000/docs')" || exit 1

CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]
