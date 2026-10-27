# DataSleuth

Connector-based AI data investigation platform. Connect live sources, ask questions, and inspect evidence-backed results without loading entire datasets into the application or the LLM.

Numbers are computed by SQL/Python. The model only plans and explains. Failed sources are never replaced with dummy data.

## Run locally

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
pip install -r backend/requirements.txt
Copy-Item .env.example .env
$env:PYTHONPATH = "$pwd\backend"
uvicorn app.main:app --reload --app-dir backend --port 8000
```

In a second terminal:

```powershell
cd frontend
npm install
npm run dev
```

Open http://localhost:5173

Optional LLM planning: set `LLM_PROVIDER=openai` and `LLM_API_KEY` in `.env`. Without a key, a schema-aware heuristic planner still runs.

## What is in the MVP

- File connector: CSV, JSON, Parquet via DuckDB (projection, filters, aggregations)
- PostgreSQL connector: read-only session, SQL pushdown, statement timeout, EXPLAIN metrics when permitted
- REST connector: HTTPS only, pagination, response limits, SSRF blocks
- Schema discovery and bounded profiling
- Structured query plans, policy validation, cancellation, provenance, report export
- React investigation UI

## Tests

```powershell
$env:PYTHONPATH = "$pwd\backend"
pytest backend/tests -q
```
