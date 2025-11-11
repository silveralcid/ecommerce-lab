# 🧩 eCommerce Lab Project Tracker

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

## 🧠 Notes
- Each branch is **independent**, runnable with its own `main.py` or `server.py`.  
- Use **FastAPI** or **asyncio TCP** depending on focus area.  
- Each project includes:
  - Local run command (`make run file=main.py`)
  - Minimal README per branch (overview, endpoints, test notes)
  - Unit tests (`pytest` + `pytest-asyncio`)
- The final branch `combined-demo` integrates key features from all slices.

### Suggested Workflow
```bash
# Start new project slice
git checkout -b cart-pricing main

# Work, commit, and test
make lint
make test

# Merge into combined demo later
git checkout combined-demo
git merge cart-pricing
````

**Goal:** Build a cohesive understanding of distributed e-commerce platform design patterns — one small, testable system at a time.

