# Contributing to VulneraX

Thanks for helping improve VulneraX. Here is how we collaborate:

## 1. Understand the stack
- Backend agents: Python FastAPI/Flask services under `agent/`, including the orchestrator (`agent/orchestrator/agent.py`) and scanners (`zap.py`, `simple_scanner.py`, `enhanced_scanner.py`).
- Frontend : `streamlit/streamlit_app.py` renders agent reports and offers PDF exports.
- Helpers: `common/a2a_*` for agent-to-agent communication and `aventus/recon_agent` for Google ADK orchestrations.

## 2. Development workflow
1. Fork/checkout the repo and create a feature branch (`feature/descriptive-name`).
2. Install dependencies:`pip install -r requirements.txt`
3. Make atomic commits describing the intent (e.g., `feat: add payload report export`).
4. Run relevant tests/lints locally before pushing.
5. Open a pull request, describe what you changed, reference any issues, and assign reviewers.

## 3. Coding standards
- Follow existing style: keep formatting consistent with surrounding files.
- Add short comments for complex logic; avoid redundant comments.
- Keep README/Docs updated when adding behaviour or APIs.

## 4. Testing & validation
- Python: manually run relevant scripts (e.g., `python agent/server.py`, `uvicorn agent.main:app --reload`) and ensure API responses behave as expected.
- Streamlit: `streamlit run streamlit/streamlit_app.py` and confirm reports/buttons behave.

## 5. Reporting issues
- Open new GitHub issues for bugs, missing features, or documentation gaps.
- Provide reproduction steps, stack traces, environment details (OS, Python/node versions).
- Label issues appropriately (e.g., `bug`, `enhancement`, `infra`).



