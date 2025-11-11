
# Echo & Validation Gateway

This slice is the first step in the **eCommerce Platform Lab**.  

It builds a standalone **Echo & Validation Gateway** that can run in isolation and later integrate into the main branch as a reusable service module.



## Overview

The gateway acts as a **validation and observability layer** that accepts JSON input, validates it against a predefined schema, and responds in a standardized format.  
You will first implement a **TCP server**, then extend it with an **HTTP interface** using FastAPI and Uvicorn.

The design must prioritize:
- Isolation: each door (TCP/HTTP) runnable and testable by itself.
- Reusability: shared validation, metrics, and config logic in a core package.
- Observability: structured logs, latency tracking, and simple metrics counters.
- Framework-agnostic core logic: validation and metrics must work in both modes.



## Goals

1. Build a **TCP server** that:
   - Listens on a configurable port.
   - Receives line-delimited JSON messages.
   - Validates payload structure.
   - Returns the same JSON or a structured error object.
   - Logs latency and metrics to stdout.

2. Refactor into a **modular package layout**:
```

echo_gateway/
├── core.py          # shared validation + metrics
├── tcp_server.py    # standalone TCP implementation
├── http_server.py   # (to be added later)
├── config.py        # environment config & feature flags
└── **init**.py

````

3. Extend to an **HTTP door**:
- Expose `/echo` endpoint.
- Use shared core logic.
- Add `/metrics` for internal monitoring.

4. Write **tests** for core and TCP logic.



## Functional Requirements

### Validation Rules
- Input must be a JSON object containing:
```json
{ "message": "string", "user_id": "string|int", "timestamp": "ISO-8601 string" }
````

* On success, return the same payload.
* On failure, return:

  ```json
  {
    "error": {
      "code": "VALIDATION_ERROR",
      "details": ["..."]
    }
  }
  ```

### Logging

* Output structured logs per request:

  ```json
  {
    "client": "127.0.0.1",
    "status": 200,
    "latency_ms": 1.42,
    "metrics": {"requests": 3, "success": 2, "errors": 1}
  }
  ```

### Metrics

* Maintain counters for `requests`, `success`, and `errors`.
* Must be thread-safe.



## Technical Constraints

* **Language:** Python 3.10+
* **Concurrency:** `threading` or `asyncio` as appropriate
* **TCP Server:** Must use Python stdlib (`socket`, `threading`)
* **HTTP Server:** Use `FastAPI` + `Uvicorn`
* **Validation:** Use `pydantic` in HTTP layer; custom validator in TCP layer
* **Data storage (optional):** `aiosqlite` or `redis` for future persistence
* **Configuration:** Use environment variables or `.env` via `python-dotenv`


## Testing Requirements

Use `pytest` and `pytest-asyncio`:

* Validate correct responses for valid/invalid inputs.
* Confirm metrics increment correctly.
* Check proper handling of malformed JSON.
* Ensure concurrent connections do not corrupt shared state.


## Deliverables

1. **Working TCP server** (`echo_gateway.tcp_server`)
   Runnable via:

   ```bash
   python -m echo_gateway.tcp_server
   ```

2. **Core validation and metrics module** (`echo_gateway.core`)

3. **HTTP door extension** (`echo_gateway.http_server`)
   Runnable via:

   ```bash
   uvicorn echo_gateway.http_server:app --reload
   ```

4. **Tests** covering both core and TCP logic (`tests/`)

5. **README.md** documenting setup, usage, and testing.


## Evaluation Criteria

| Area              | Description                                                                    |
| ----------------- | ------------------------------------------------------------------------------ |
| **Correctness**   | TCP server runs, validates input, and returns correct output.                  |
| **Structure**     | Package layout follows modular design.                                         |
| **Extensibility** | HTTP door integrates without breaking core.                                    |
| **Testing**       | Unit and integration tests exist for core and TCP logic.                       |
| **Observability** | Logs and metrics are implemented and accurate.                                 |
| **Code Quality**  | Follows PEP 8; uses type hints; passes `flake8`, `black`, `isort`, and `mypy`. |


## Environment Setup

```bash
python -m venv .venv
source .venv/bin/activate  # or .venv\Scripts\activate on Windows

pip install -r requirements.txt requirements-dev.txt
```

**Dependencies:**

```text
# =========================
# Core dependencies
# =========================

fastapi
uvicorn[standard]
aiohttp
websockets
aiosqlite
redis
pydantic
python-dotenv

# =========================
# Development & testing
# =========================

pytest
pytest-asyncio
pytest-cov
black
flake8
isort
mypy
pre-commit
```


## Expected Timeline

| Phase | Deliverable                   | Duration |
| ----- | ----------------------------- | -------- |
| 1     | TCP Echo Gateway (standalone) | 1 hr     |
| 2     | Core module & tests           | 1 hr     |
| 3     | HTTP integration & polish     | 1 hr     |



## Notes

* Do **not** rely on external frameworks for TCP.
* The **HTTP door** must reuse existing logic from `core.py`.
* Write code as if this will be merged into a larger system — no hardcoded values.
* Favor readability and resilience over brevity.


