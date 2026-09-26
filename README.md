# pulse-ops

> Ecosystem: [api](https://github.com/RakhaYandra/pulse) · [web](https://github.com/RakhaYandra/pulse-web) · [docs](https://github.com/RakhaYandra/pulse-docs/releases) · [data](https://github.com/RakhaYandra/pulse-data) · [qa](https://github.com/RakhaYandra/pulse-qa) · [ops](https://github.com/RakhaYandra/pulse-ops)

Operations for [Pulse](https://github.com/RakhaYandra/pulse): compose stack,
observability, and runbooks. No application code lives here.

## Contents

| Path | Purpose |
|---|---|
| `docker-compose.yml` | Full stack: postgres, redis (AOF), api, scheduler, worker-1..4, bench stub |
| `docker-compose.observability.yml` | Prometheus + Grafana + 2 alerts |
| `.env.example` | All env with safe dev defaults (real `.env` never committed) |
| `observability/` | Prometheus, alerts, Grafana provisioning + dashboard JSON |
| `RUNBOOK.md` | Bring-up, services, daily ops |
| `SLA.md` | Measured targets (from `pulse-qa` bench evidence) |
| `TROUBLESHOOTING.md` | Real incidents + short list |
| `ops/BACKUP.md` | pg_dump, volume snapshots, housekeeping cadence |
| `ops/SELFMONITORING.md` | Dogfooding procedure (Pulse watches its own `/health`) |

Start here: [`RUNBOOK.md`](RUNBOOK.md).
