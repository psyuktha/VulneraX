# VulneraX

VulneraX is an autonomous Red Team platform that orchestrates AI-driven reconnaissance, payload generation, attack execution, and adaptive reporting. It combines Python-based scanning/orchestrator agents so you can trigger ethical hacking workflows, inspect results, and export polished PDFs without stitching together multiple services.

## Architecture
- `agent/`: Python services (FastAPI + Flask) that expose `/scan`/`/execute`, orchestrate Google ADK agents (`recon_agent`, `payload_agent`, `attack_agent`, `report_agent*`), and contain scanners (`SimpleScanner`, `EnhancedSecurityScanner`, `SecurityScanner`) backed by OWASP ZAP.
- `client/`: Next.js 14 (`app/` router) frontend with a landing page, protected dashboard, scan form, re-usable UI atoms (`components/ui`), and an API route that proxies scan requests to the backend (`app/api/scan/route.ts`).
- `streamlit/`: A dark-themed Streamlit experience that loads text reports from `agent/reports`, renders them in-app, and lets you download PDF exports via ReportLab.
- `common/`: Lightweight helpers for agent-to-agent communication (`a2a_client.py`, `a2a_server.py`).
- `aventus/`: A Google ADK recon agent implementation that can be executed programmatically via `aventus/recon_agent/task_manager.py`.
- `agent/scan_results/`: Scan artifacts such as `endpoints.json`, `structure.json`, `subdomains.json` and provides data consumed by Enhanced scanners and report agents.

## Getting Started
1. **Install Python dependencies**
   ```bash
   python -m pip install -r requirements.txt
   ```
   > Ensure OWASP ZAP is running on `http://localhost:8080`. 

2. **Backend**
   - Run FastAPI orchestrator:
     ```bash
     cd agent
     uvicorn main:app --reload --host 0.0.0.0 --port 8000
     ```

3. **Streamlit report viewer**
   ```bash
   streamlit run streamlit/streamlit_app.py
   ```
   Use the “Scan Now” button to open the backend, read `agent/reports/attack_report.txt` and `attack_report2.txt`, and download PDF versions.

## Scan Workflow
1. The frontend sends `POST /api/scan` with `{ input: "Scan <URL>" }`.
2. The proxy forwards the body to the FastAPI backend, which invokes `run_security_scan()` from `agent/orchestrator/agent.py`.
3. Scanners (`SecurityScanner`, `SimpleScanner`, `EnhancedSecurityScanner`) discover endpoints, forms, and subdomains, storing results under `agent/scan_results`.
4. Payload and attack agents optionally generate curl commands that `execute()` runs via `subprocess`.
5. Reports are saved via `save_report_to_file()` so Streamlit and export scripts can reuse them.

## Directory Snapshot
- `agent/`: orchestrator, scanners, reports, scan results
- `client/`: Next.js app with `app/` routes, UI components, and API proxy
- `streamlit/streamlit_app.py`: report viewer + PDF exporter
- `common/`: helper agents (a2a)
- `aventus/`: Google ADK recon agent implementation
- `requirements.txt`: Python dependencies
- `package.json`/`package-lock`: Next.js dependencies

## Monitoring & Logs
- `agent/scan_results/*.json` contain endpoints, forms, and structure data extracted during scans.
- Each scanner logs to console/file (see `SimpleScanner` and `SecurityScanner` loggers) for auditing.
- Reports live in `agent/reports/attack_report.txt` and `attack_report2.txt`.


## License
See [LICENSE](LICENSE).

