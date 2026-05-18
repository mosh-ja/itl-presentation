---
layout: section
---

# Concurrency Viewpoint

---
layout: two-cols
class: text-sm
---

## Services

* All services are async

* Multiple pods per service

* SQS message lifecycle:
    * Receive → process → delete on success
    * On failure or timeout → retry → DLQ

::right::

<div style="padding-left: 0.5rem;">

## Scheduling

* Dagster shared daemon and one agent per environment

* Parallel runs across environments and within the same environment

* Athena quotas determine the actual level of parallelism

</div>
