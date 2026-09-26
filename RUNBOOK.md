# Pulse Ops Runbook

Day-to-day operations for the [Pulse](https://github.com/RakhaYandra/pulse)
stack. All commands run from this repo root.

## Bring-up

```bash
cp .env.example .env   # set JWT_SECRET (required — boot fails without it)
docker compose up -d --build
curl localhost:8080/api/v1/health   # {"status":"healthy",...}
```

Dashboard: `pulse-web` repo (`npm run dev`, `:5173`).

## Services

| Service | Mode/role | Metrics |
|---|---|---|
| `postgres` | state (volume `pgdata`) | — |
| `redis` | queue + rate budgets (AOF, volume `redisdata`) | — |
| `api` | REST + auth | `:9101` |
| `scheduler` | due-scan → enqueue | `:9105` |
| `worker-1..4` | check → store → incident eval | `:9102-04/06` |
| `stub` | bench target (`--profile bench` only) | — |

Workers are explicit services (not replicas) so each `/metrics` has a fixed
port. Scale down unused workers: `docker compose stop worker-3 worker-4`.

## After editing compose env: `up -d`, never bare `start`

`start` resumes stale containers with old env (see pulse-docs
POSTMORTEM-002). `up -d` recreates on config change.

## Observability

```bash
docker compose -f docker-compose.yml -f docker-compose.observability.yml up -d prometheus grafana
# Grafana :3000 (admin / $GF_SECURITY_ADMIN_PASSWORD), dashboard "Pulse"
```

Alerts: `PulseQueueBacklog` (queue > 500, 5m), `PulseProcessErrors` (> 0.1/s, 5m).

## Housekeeping

- Bench/e2e leftovers: `bench/clean.sql` lives in `pulse-qa`; run after every
  bench or e2e batch, then `redis-cli FLUSHDB`.
- Backup/restore: [`ops/BACKUP.md`](ops/BACKUP.md).
- Self-monitoring: [`ops/SELFMONITORING.md`](ops/SELFMONITORING.md).
