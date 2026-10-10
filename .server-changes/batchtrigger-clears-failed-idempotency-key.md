---
area: webapp
type: fix
---

`batchTrigger` now re-triggers when the idempotency key points to a run
that already ended in a terminal failure state
(`CRASHED` / `COMPLETED_WITH_ERRORS` / `SYSTEM_FAILURE` / `INTERRUPTED` /
`TIMED_OUT` / `EXPIRED`), matching the single-trigger behaviour. Previously
a batch call with the same key as a dead run silently returned the dead
run as `isCached: true`, leaving the caller waiting on a run that would
never produce output. Multiple batch items that share the same key now
also share the same replacement run id, so the response no longer
contains ghost ids pointing at runs that were never created. Closes #4819.
