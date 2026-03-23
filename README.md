# AI Workflow Deviation Detector

A production-style AI operations project for detecting workflow deviations, bottlenecks, SLA breaches, handoff risks, and process anomalies from event-log CSV files.

## Why this project matters

Companies run workflows everywhere: support tickets, approvals, onboarding, service operations, compliance, and incident handling. These workflows often drift away from the intended path and create hidden delays. This project turns raw event logs into an operational intelligence dashboard.

## Core capabilities

- Event-log ingestion from CSV
- Dominant path detection
- Workflow variant analysis
- Deviation case detection
- SLA breach analysis
- Stage bottleneck ranking
- Cross-team handoff risk detection
- Transition edge view for flow analysis
- Clean dashboard UI with modern visuals
- Sample dataset included for instant demo

## Expected CSV schema

Default column names:

- `case_id`
- `timestamp`
- `activity`
- `actor`
- `duration_hours`
- `status`

All mappings are configurable from the UI.

## Local run

```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
uvicorn app.main:app --reload
```

Then open `http://127.0.0.1:8000`.

## API endpoints

- `GET /` - UI
- `GET /health` - service health
- `GET /api/sample` - run analysis on bundled sample workflow
- `POST /api/analyze` - upload CSV and analyze a custom workflow

## Demo workflow ideas

This project can be adapted to:

- customer support workflows
- procurement approvals
- IT service requests
- onboarding pipelines
- compliance review flows
- operations escalations

## Next production upgrades

- persistent run history with SQLite or Postgres
- workflow comparison across time windows
- role-based dashboards
- PDF export for reports
- anomaly alerts via webhook
- LLM-generated narrative summaries
- process conformance scoring against a reference workflow


## UX behavior

- The app starts in an empty state.
- No data is analyzed until you upload a CSV or explicitly click **Load demo dataset**.
- The bundled demo file lives at `data/sample_workflow.csv` and is only used in demo mode.


## Production-style ingestion notes

- Uploads are parsed independently. No implicit merging is performed.
- ZIP archives are expanded recursively, including nested ZIP files and nested `.xes.gz` event logs.
- Supported dataset types: CSV, Excel, XES, XES.GZ, ZIP.
- Multi-file and folder uploads are analyzed through `/api/analyze-multiple`.
- Parsing errors are surfaced explicitly as `Failed to parse dataset: <filename>` style failures instead of falling back to demo data.
- Basic column auto-detection is included for common aliases such as `case:concept:name`, `time:timestamp`, `concept:name`, and `org:resource`.
