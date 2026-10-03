# 🚀 Production Readiness Checklist

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Get the Full Kit on Gumroad](https://img.shields.io/badge/Full_Kit-Gumroad-$19-blue?style=for-the-badge&logo=gumroad)](https://kython.gumroad.com/l/rstpsb)

A pre-flight checklist for engineering teams, tech leads, and solo developers to
verify reliability, security, and observability before going live.

The baseline below is free and open-source under MIT. If your team wants the
full operational system — deployment runbooks, incident matrices, post-mortem
worksheets, and more — see the [Gumroad kit](https://kython.gumroad.com/l/rstpsb).

---

## Pre-Flight Production Readiness Review (PRR)

### 1. Architecture, Capacity & Scaling
- [ ] **Capacity Baseline:** Expected peak requests per second (RPS) and concurrency documented.
- [ ] **Horizontal Auto-scaling:** Auto-scaling / HPA policies configured with defined min/max bounds.
- [ ] **Resource Limits:** CPU and Memory requests/limits set on all container specs (prevents OOM kills).
- [ ] **Graceful Termination:** Application traps `SIGTERM`, halts incoming traffic, and finishes in-flight requests within timeout budget (e.g. 30s).
- [ ] **Connection Pooling:** DB and HTTP connection pools sized relative to worker thread counts (avoids connection exhaustion).

### 2. Observability & Telemetry
- [ ] **Structured Logging:** Standardized JSON formatting including `timestamp`, `level`, `trace_id`, and `service_name`.
- [ ] **PII Scrubbing:** Verified no passwords, auth tokens, session cookies, or PII appear in standard stdout logs.
- [ ] **Health Endpoints:**
  - [ ] `/healthz` or `/live` (Liveness: process is up)
  - [ ] `/ready` (Readiness: upstream dependencies, DB, caches reachable)
- [ ] **Core Metrics (RED Method):**
  - [ ] **Rate:** Requests per second aggregated by endpoint and HTTP status code
  - [ ] **Errors:** Error rate percentage alerts triggered at >1% over a 5-minute window
  - [ ] **Duration:** P50, P95, P99 latency instrumentation on primary workflows
- [ ] **Dashboards & Alerts:** Primary monitoring dashboard created; alerts routed to target on-call channel.

### 3. Resilience, Fault Tolerance & Networking
- [ ] **Explicit Timeouts:** Every outbound network call (third-party APIs, DBs, caches) has an explicit timeout (recommended: ≤ 3s).
- [ ] **Retry Strategy:** Retries use exponential backoff with jitter; restricted strictly to idempotent operations.
- [ ] **Circuit Breakers / Fallbacks:** Graceful fallback behavior defined if upstream dependencies go down.
- [ ] **Rate Limiting:** Public ingress protected with token-bucket or sliding-window rate limiters.
- [ ] **CORS Configuration:** Explicit origin whitelist configured; wildcards (`*`) forbidden on authenticated endpoints.

### 4. Security & Compliance
- [ ] **Secrets Segregation:** No credentials, API tokens, or secrets committed in code or standard `.env` files (pulled from a secret manager at runtime).
- [ ] **Least Privilege IAM:** Cloud IAM permissions scoped strictly to target resources (no wildcards `*` on write/delete actions).
- [ ] **Automated Vulnerability Scan:** Dependency audit tool (Trivy, Snyk, Dependabot) reports zero Critical or High vulnerabilities.
- [ ] **TLS Everywhere:** Enforced TLS 1.2+ for external and internal service-to-service transit.

### 5. Deployment, Backups & Rollback
- [ ] **Rollback Tested:** Rollback script/command verified in staging (under 5 minutes to restore previous image).
- [ ] **Zero-Downtime Strategy:** Rolling updates or Blue/Green deployment configuration verified.
- [ ] **Automated Backups:** Point-In-Time-Recovery (PITR) enabled on primary datastore with validated automated snapshot retention.
- [ ] **Runbook Linked:** Operational runbook documented and accessible to all on-call engineers.

---

## Beyond this checklist

The above covers a single pre-flight review. The
[full Production-Ready Engineer Kit on Gumroad](https://kython.gumroad.com/l/rstpsb) ($19)
adds the templates you actually use during and after launch:

- **Zero-Downtime Deployment Guide** — pre, during, and post-release verifications
- **Database Migration Safeguards** — Expand/Contract pattern that avoids table locks
- **Incident Severity Matrix & Response SLAs** — SEV-1 through SEV-4 definitions
- **On-Call Triage Runbooks & Comms Templates** — status page + executive snippets
- **Blameless Post-Mortem System** — 5-Whys worksheet and systemic root-cause framework
- **SLI / SLO Calculator & Service Catalog Spec** — standard error-budget policies

## Contributing

Have a real-world production failure check to add? Pull requests welcome — please
keep additions concrete and battle-tested, not theoretical.

## License

MIT. Free to use across personal and internal commercial projects.