---
layout: section
---

# Concurrency Viewpoint

---
layout: two-cols
class: text-sm
---

## Services

* Streaming ingestion, batch processing

* Data is partitioned by hour in the lake

* All services are async, multiple pods per service

* dbt (current): runs every 2 hours, reprocessing the last 2 days

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
