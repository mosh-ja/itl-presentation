---
layout: section
---

# Concurrency Viewpoint

---
layout: two-cols
class: text-sm
---

## Services

- All services are async.

- Multiple pods per service.

**SQS message lifecycle:**
- Receive → process → delete on success
- On failure: visibility timeout expires → message reappears → retry
- After max retries → DLQ

::right::

<div style="padding-left: 2rem;">

## Scheduling

- Dagster **daemon** (1 shared) + **agent** per env → parallel runs across envs and within the same env.

- Athena quota determines the actual parallelism.

</div>
