# Pulse Troubleshooting

Real incidents first (full stories in `pulse-docs` postmortems), then the
short list.

## Workers/scheduler exited(1): `JWT_SECRET is required`

JWT is required in api mode only. If workers die with this error, they were
built from a revision where the composition root demanded the secret
everywhere — rebuild from current main. Never copy the secret into worker
env to "fix" it (least privilege).

## Bench checks all ERROR: `host resolves to blocked address: stub`

Workers run without `PULSE_ALLOW_HOSTS=stub` (stale containers from bare
`start`, or env missing). Fix: `export PULSE_ALLOW_HOSTS=stub` +
`docker compose up -d worker-N` (recreate, not start).

## Queue exploding (depth ≫ N)

Pre-dedup symptom. On current main the Lua claim + release bounds the queue;
confirm `scheduler_skipped_total` is moving. If depth still grows, check for
stuck workers (`worker_jobs_inflight` flat at max) and "queue pop failed" logs.

## Orphan containers (`NAME-1` vs `NAME-1-1` duplicates)

Stale Compose state after file edits + `stop`/`start` cycles.
`docker compose down --remove-orphans`, then `up -d`.

## 429 on scripts

Auth 10/min/IP, API 100/min/IP (Redis-backed, fail-open). Bench `run.sh`
retries register with backoff; ad-hoc scripts hitting 429 should do the same.

## API 500 on `/reports/reliability`

Was: typed-int `||` text in SQL (`make_interval(days => $2)` is the fix).
If a new 500 appears, check api logs + reproduce the query in psql first.

## Fresh divorce test

`docker compose down -v` wipes everything (volumes included). Rebuild from
zero proves the stack is reproducible — do this before calling any release
"done".
