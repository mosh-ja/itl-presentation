---
layout: section
---

# Concurrency Viewpoint

---
layout: default
class: text-sm
---

## Runtime Process Model

| Service | Concurrency Unit | Coordination |
|---------|-----------------|-------------|
| **DESA** | Async RabbitMQ consumer per replica | AMQPS acknowledgement |
| **Events Transformation** | Async SQS long-poll; per-S3-object handler | SQS visibility timeout |
| **TEDI** | Two async SQS consumer loops in one service | SQS visibility timeout; internal SQS hand-off |
| **Relational Processor** | Async SQS long-poll; synchronous Pandas per object | SQS visibility timeout |
| **DRT** | Async SQS; per-message async dispatch | SQS visibility timeout; async Athena polling |
| **DDB Replicator** | Async SQS; `asyncio.gather` for parallel table merges | Athena query polling |
| **Dagster daemon** | Single scheduler process; built-in cron ticks | PostgreSQL run storage |
| **Dagster agent** | Long-running code-location process; outbound poll | gRPC/HTTPS to control plane; spawns dbt subprocess |

<br>

> All SQS services share a common pattern from `common-modules`: receive batch → async handler → delete on success. Failed messages reach DLQ after max receive count.

---
layout: default
---

## Scheduling

| Scheduler | Target | What |
|-----------|--------|------|
| **Dagster** (built-in cron) | Dagster agent (per slice) | dbt daily partition materialization |
| **Dagster** (built-in cron) | Dagster agent (per slice) | aws_ops (Iceberg maintenance) |
| **EventBridge** (hourly) | DRT SQS | `data_swamp_sql_run` |
| **EventBridge** (daily) | DRT SQS | `data_swamp_sql_run` (daily) |
| **EventBridge** (scheduled) | DDB Replicator SQS | CDC invocation |

<br>

> EventBridge rules for `dbt_command_execution` and `aws_ops` are **disabled but not deleted** in Terraform — rollback path if Dagster fails.
