---
layout: section
---

# Functional Viewpoint

---
layout: default
class: text-sm
---

## Functional Elements

| Element | Repo | Responsibility |
|---------|------|---------------|
| **DESA** | `core-services` | Consume RabbitMQ events; envelope & deliver to raw S3 via Firehose |
| **Events Transformation** | `data-infra` | Normalize v1/v2 schemas to `GenericEvent`; publish to SNS |
| **SNS Routing Layer** | Infrastructure | Fan-out transformed events via message attribute filters |
| **TEDI** | `data-infra` | Hash structured PHI; NLP-mask free-text PHI via Comprehend Medical |
| **Relational Processor** | `data-infra` | Convert JSON events to Parquet; group by partition dimensions |
| **DRT** | `data-infra` | Run Data Swamp SQL; data retention churn |
| **Dagster** | `data-infra` | Schedule & execute dbt builds and aws_ops across all 5 slices |
| **DDB Replicator** | `data-infra` | Merge DynamoDB CDC + full-load exports into Glue/Iceberg via Athena MERGE |

---
layout: default
---

## Pipeline Flow

<Transform :scale="0.68">

```mermaid
graph TD
    RMQ["RabbitMQ\n(External)"]
    DDB_Ext["DynamoDB\n(External)"]
    EB["EventBridge\n(data swamp + DDB CDC)"]
    GHA["GitHub Actions\nOps"]
    Dagster_F["Dagster\n(dbt + aws_ops)"]

    DESA["DESA"]
    Raw["S3 Raw\n(PHI)"]
    ET["Events Transformation"]
    SNS["SNS Processing Hub"]
    TEDI["TEDI\n(PHI De-identification)"]
    nSilver["S3 nSilver\n(no-PHI)"]
    Silver["S3 Silver\n(PHI)"]
    RP["Relational Processor"]
    nGold["S3 nGold\n(no-PHI Parquet)"]
    Gold["S3 Gold\n(PHI Parquet)"]
    DRT["DRT\n(Data Swamp + Churn)"]
    DDBR["DDB Replicator"]
    Athena["Athena / Glue\nAnalytical Layer"]

    RMQ --> DESA --> Raw --> ET --> SNS
    SNS -->|"PHI path"| Silver
    SNS -->|"text-DI filter"| TEDI
    SNS -->|"no-PHI direct"| nSilver
    TEDI --> nSilver
    Silver --> RP
    nSilver --> RP
    RP --> Gold
    RP --> nGold
    nGold --> Athena
    DRT --> Athena
    Dagster_F --> Athena
    DDB_Ext --> DDBR --> Athena
    EB --> DRT
    EB --> DDBR
    GHA --> DRT
    GHA --> DDBR
    GHA -->|"API calls"| Dagster_F
```

</Transform>
