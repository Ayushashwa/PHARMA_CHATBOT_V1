# Pharma Data Chatbot (Learning Project)

A minimal FastAPI + Hugging Face Router chatbot that evolves into a dataset-aware pharma assistant.

## Setup

1) Create and activate a virtual environment:

```bash
python3 -m venv .venv
source .venv/bin/activate
```

2) Install dependencies:

```bash
pip install -r requirements.txt
```

3) Create your local `.env` (never commit it):

```bash
cp .env.example .env
```

Then edit `.env` and set your Hugging Face token:

```
HF_TOKEN=your_real_token_here
```

## Run the server

```bash
python -m app.main
```

The server uses `PORT` from `.env` (default 8000).

## Open the chat UI

Visit:

```
http://localhost:8000
```

## KPIs

- **sales_total:** Total sales across the dataset (sales = trx).
- **sales_per_mdm_overall:** Total sales divided by unique MDM count.
- **sales_per_mdm_by_region:** For each region: (sum of trx in region) / (unique MDM_ID in region).

Example prompts:

- "sales per mdm overall"
- "sales per mdm overall in 2024-01"
- "sales per mdm by region"
- "sales per mdm by region in 2024-01"

## Coverage, Sales Efficiency, and Field Productivity KPIs

Definitions (per month):

- **total_mdm:** Count of distinct MDM_ID present in the month.
- **reached_mdm:** Count of distinct MDM_ID where calls > 0.
- **not_reached_mdm:** total_mdm - reached_mdm.
- **reach_percent:** (reached_mdm / total_mdm) * 100.
- **zero_call_percent:** (not_reached_mdm / total_mdm) * 100.
- **total_sales:** Sum of trx (sales).
- **total_calls:** Sum of calls.
- **sales_per_call:** total_sales / total_calls.
- **sales_per_reached_mdm:** total_sales / reached_mdm.
- **calls_per_reached_mdm:** total_calls / reached_mdm.

Business meaning:

- **Coverage KPIs:** Show how much of the monthly universe is being reached.
- **Sales Efficiency KPIs:** Show how effectively calls convert to sales.
- **Field Productivity KPIs:** Show effort per reached MDM and sales per reached MDM.
- **Region/Ecosystem KPIs:** Show where coverage and efficiency are strongest or weakest.

API endpoints:

- `/api/kpi/coverage?month=YYYY-MM`
- `/api/kpi/efficiency?month=YYYY-MM`
- `/api/kpi/region?month=YYYY-MM`
- `/api/kpi/ecosystem?month=YYYY-MM`

Example chat prompts:

- "coverage in jan 2024"
- "reach percent in 2024-01"
- "sales per call in 2024-01"
- "sales per reached mdm in feb 2024"
- "region-wise sales efficiency in 2024-01"
- "ecosystem coverage in 2025-12"

## KPI Dictionary

The KPI catalog (definitions, formulas, and example questions) is available at `/api/kpis` and shown in the UI under “KPI Dictionary.”

## Reach KPIs

Definitions (per month):

- **reached_mdm:** Count of distinct MDM_ID where calls > 0.
- **total_mdm:** Count of distinct MDM_ID present in the month.
- **not_reached_mdm:** total_mdm - reached_mdm.
- **reach_percent:** (reached_mdm / total_mdm) * 100.

API endpoints:

- `/api/reach?month=YYYY-MM`
- `/api/reach/all`

Example prompts:

- "reached mdm in 2024-01"
- "not reached mdm in 2024-01"
- "mdm coverage in 2025-12"

## Learning Path

- **Milestone 1 — Test Bot (HF working):** Learn how to call Hugging Face Router with the OpenAI SDK and wire a simple FastAPI chat endpoint.
- **Milestone 2 — Add Pharma CSV Loading:** Learn how to load a CSV with pandas and expose summary endpoints for exploration.
- **Milestone 3 — Hybrid Chat (Rules + Data Answers):** Learn lightweight intent routing: answer from data when possible, otherwise fall back to the LLM with a strict dataset rule.
