---
layout: section
---

# Development Viewpoint

---
layout: default
class: text-sm
---

## Functional → Platform

| **Functional concept** | **Runtime** |
| ------------------ | ------- |
| Pipeline microservices (DESA, DET, TEDI, …) | Python on EKS |
| Service handoff | SQS — message body or S3 path for large payloads |
| CDC replication (Postgres) | DMS |
| CDC replication (DynamoDB) | Kinesis Data Streams → Firehose |
| Entity materialization | Python microservice (EKS · SQS) |
| Data transformation | dbt on Athena |
| Scheduler (current) | EventBridge |
| Scheduler (target) | Dagster — control plane + agent |
| Lake permissions | IAM · Lake Formation |

---
layout: default
class: text-sm
---

## Transformation — Current vs Target

<div class="grid grid-cols-2 gap-6" style="margin-top: 0.5rem;">

<div>

**Current**

<div class="mermaid-center" style="transform: scale(0.75);">

```mermaid
%%{init: {
  'theme': 'base',
  'themeVariables': {
    'background': '#0B0F14',
    'primaryColor': '#1e2a38',
    'primaryBorderColor': '#3a5068',
    'primaryTextColor': '#dce6f0',
    'lineColor': '#dce6f0',
    'edgeColor': '#7ab8e0',
    'edgeLabelBackground': '#1e2a38'
  }
}}%%
graph TD
    SRC["Sources"]
    EB["EventBridge"]
    dbt["dbt"]
    ATH["Athena"]
    OUT["Fact & Dim"]

    SRC --> dbt
    EB -->|launch| dbt
    dbt --> ATH --> OUT

    classDef bad stroke:#aa4444
    class EB,dbt bad
```

</div>

</div>

<div>

**Target**

<div class="mermaid-center" style="transform: scale(0.75);">

```mermaid
%%{init: {
  'theme': 'base',
  'themeVariables': {
    'background': '#0B0F14',
    'primaryColor': '#1e2a38',
    'primaryBorderColor': '#3a5068',
    'primaryTextColor': '#dce6f0',
    'lineColor': '#dce6f0',
    'edgeColor': '#7ab8e0',
    'edgeLabelBackground': '#1e2a38'
  }
}}%%
graph TD
    SRC["Sources"]
    CP["Dagster\nControl Plane"]
    AG["Dagster Agent"]
    dbt1["dbt 1"]
    dbt2["dbt 2"]
    dbtN["dbt N"]
    ATH["Athena"]
    OUT["Fact & Dim"]

    SRC --> dbt1
    SRC --> dbt2
    SRC --> dbtN
    CP --> AG
    AG --> dbt1
    AG --> dbt2
    AG --> dbtN
    dbt1 --> ATH
    dbt2 --> ATH
    dbtN --> ATH
    ATH --> OUT

    classDef good stroke:#44aa44
    class CP,AG,dbt1,dbt2,dbtN good
```

</div>

</div>

</div>

---
layout: default
class: text-sm
---

## Repositories

* **`data-infra`**
  * Services + dbt + Dagster code location
  * Each service has its own `pyproject.toml` and `Dockerfile`
  * GitHub Actions are used as ops tools (e.g. `trigger_ddb_replicator`)

* **`terraform-live`**
  * Centralized Infrastructure as Code (IaC)
  * One folder per env slice
  * Use internal terraform-modules for reuse
