<a id="readme-top"></a>

<div align="center">
  <h1>🐍 PyBase</h1>
  <p><em>A clean, framework-agnostic Python project scaffold for professional development and rapid prototyping.</em></p>

  <p>
    <a href="https://github.com/sillveralcid/pybase">View on GitHub</a> ·
    <a href="https://github.com/sillveralcid/pybase/issues">Report Bug</a> ·
    <a href="https://github.com/sillveralcid/pybase/issues">Request Feature</a>
  </p>
</div>

## 🚀 About

**PyBase** is a lightweight Python scaffold preconfigured with modern development tools:

- **black** – formatting  
- **isort** – import sorting  
- **flake8** – linting  
- **mypy** – static type checking  
- **pytest** – testing  
- **pre-commit** – automated code quality  
- **Makefile** – consistent developer commands (`make lint`, `make test`, etc.)

Use this as your base for any Python project — from quick scripts to FastAPI or Django applications.

## 🧩 eCommerce Lab Projects

Each mini-project represents a self-contained backend service slice exploring a specific e-commerce platform concept.  
Every branch should run and be tested in isolation. The final project (`combined-demo`) merges all components into one unified system.

| Branch | Project Theme | Core Use Case | Key Concepts Practiced |
|---------|----------------|----------------|-------------------------|
| `echo-gateway` | **Echo & Validation Gateway** | JSON echo endpoint that validates schema and returns standardized errors. | Input validation, structured error bodies, latency logging, metrics counters. |
| `cart-pricing` | **Cart & Pricing API** | Simple cart that totals `price*qty`, enforces `Decimal` rounding, rejects mixed currency. | Decimal math, rounding policy, money helpers, validation ordering, observability fields. |
| `catalog-pagination` | **Catalog with Pagination** | Product list with cursor pagination and filtering. | Stable sort key, cursor encoding, page size, `next_cursor=None`, latency metrics. |
| `gift-grant` | **Gift Grant (Idempotent Create)** | `POST /gift` creates a gift once per `idempotency_key`; duplicates return prior result. | Idempotency semantics, result caching, duplicate detection, 409 vs 400 handling. |
| `gift-history` | **Gift History (List + Cursor)** | `GET /gifts` paginates grants; mirrors catalog rules. | Shared validators/helpers, parity across endpoints, cursor edge cases. |
| `promo-rate-limit` | **Rate-Limited Promo API** | Endpoint that applies a promo code but throttles by user. | Token-bucket model, 429 retry-after, metrics on rejections, auth header checks. |
| `reliability-audit` | **Reliability & Audit Service** | Wrap any prior API with retry, timeout, and audit log persistence (file or SQLite). | Timeouts, retry with jitter, audit schema, correlation IDs, structured logging. |
| `cache-metrics` | **Cache Layer & Metrics Portal** | Add in-memory cache to one endpoint + export metrics via `/metrics`. | Read-through cache, TTL, hit/miss counters, Prometheus exposition format. |
| `env-config` | **Environment Config & Feature Flags** | Add configuration management, feature flags, and environment-aware settings. | `.env` loading, safe defaults, feature toggles, dependency injection patterns. |
| `combined-demo` | **Unified Commerce Demo** | Merge best pieces: cart + gift + promo + metrics + rate limit. | Composition, shared helpers, environment config, end-to-end reasoning. |


## 🧱 Getting Started

### Prerequisites
- Python ≥ 3.11 (managed via `pyenv` recommended)
- Git
- (Optional) `make` for automated commands

### Installation
```bash
# Clone the base scaffold
git clone https://github.com/silveralcid/pybase.git my_project
cd my_project

# Create and activate a virtual environment
python -m venv .venv
# On Windows (PowerShell)
.venv\Scripts\activate
# On macOS/Linux
source .venv/bin/activate

# Install dependencies
pip install -r requirements.txt -r requirements-dev.txt

# Enable pre-commit hooks
pre-commit install
````

Then verify your environment:

```bash
make lint
make test
```

If everything passes, you’re ready to start building.

### Usage for New Projects

When you clone **PyBase** for a new project:

1. Update this README — replace “PyBase” with your project’s name.
2. Change your Git remote:

   ```bash
   git remote remove origin
   git remote add origin https://github.com/<yourname>/<yourproject>.git
   git remote -v  # confirm
   ```
3. Start coding with a clean slate:

   ```bash
   make lint
   make test
   pre-commit run --all-files
   ```


## 🧩 Makefile Commands

| Command                | Description                                               |
| ---------------------- | --------------------------------------------------------- |
| `make lint`            | Run black, isort, and flake8                              |
| `make test`            | Run pytest                                                |
| `make clean`           | Remove cache and build files                              |
| `make check-tools`     | Show tool versions                                        |
| `make run file=app.py` | Run a specific script                                     |
| `make typecheck`       | Run mypy for type validation                              |
| `make coverage`        | Run pytest with coverage report (if pytest-cov installed) |

## 🧰 Toolchain Summary

| Tool           | Purpose                     |
| -------------- | --------------------------- |
| **black**      | Auto-format code            |
| **isort**      | Sort imports                |
| **flake8**     | Linting & style enforcement |
| **mypy**       | Type checking               |
| **pytest**     | Testing framework           |
| **pre-commit** | Git hooks for auto-checks   |
| **make**       | Unified command interface   |

## 🪪 License

Distributed under the MIT License.
See [`LICENSE`](LICENSE) for details.

## 👤 Contact

**Silver Alcid** - [LinkedIn](https://www.linkedin.com/in/silveralcid/) | [silveralcid@outlook.com](mailto:silveralcid@outlook.com)

<p align="right">(<a href="#readme-top">back to top</a>)</p>
