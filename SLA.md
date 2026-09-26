# Pulse SLA

Targets derived from measured runs (`pulse-qa/bench/results/*.json`), not
aspirations. Single-node scope: no HA promises.

| Signal | Target | Evidence |
|---|---|---|
| Check cadence | ~60s steady-state per monitor | N=500 run, wave spacing |
| Queue depth | max 0 sustained (dedup-bounded) | all post-hardening runs |
| Overdue monitors (>2× interval) | 0 | N=100/500/1000 runs |
| False incidents on healthy targets | 0 | ok_inc 0 in all runs |
| Timeout detection | 100% (50/50 @N=1000) | bench + engine E2E |
| API p95 local | < 100ms | Newman avg ~10ms |
| Incident notify | Telegram on OPEN/RESOLVED only | code path (live verify pending) |

Breaching `PulseQueueBacklog` or `PulseProcessErrors` alerts means the SLOs
above are at risk — see TROUBLESHOOTING.md.
