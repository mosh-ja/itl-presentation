---
layout: section
---

# Context Viewpoint

---
layout: default
---

## System Scope

<div class="grid grid-cols-2 gap-8">
<div>

### ✅ In Scope

- Real-time event ingestion from RabbitMQ
- PHI de-identification (structured + free-text)
- JSON → Parquet conversion for analytics
- DBT analytical modeling (Athena / Iceberg)
- DynamoDB table replication into the lake
- Manual operator triggers for ad-hoc ops

</div>
<div>

### ❌ Out of Scope

- Upstream RabbitMQ platform (external)
- Clinical application layer
- BI / ML tooling (Redash, Tableau)
- Commercial DynamoDB source tables
- Query interface for consumers

</div>
</div>

<br>

> **Three repositories, one platform:** `core-services` (DESA) · `data-infra` (5 pipeline services + DBT) · `terraform-live` (AWS infrastructure)

---
layout: default
---

## Context Diagram

<Transform :scale="0.72">

```mermaid
graph TD
    RabbitMQ["RabbitMQ\n(Product Event Bus)"]
    Operators["Operators\n(Platform Team)"]
    GitHub["GitHub Actions\n(CI/CD + Manual Ops)"]
    Comprehend["Amazon Comprehend Medical\n(PHI NLP)"]
    DynamoDB["DynamoDB\n(Commercial + Processing)"]
    EventBridge["AWS EventBridge\n(Scheduled Triggers)"]
    Consumers["BI / ML / Analytics\n(Downstream Consumers)"]
    ExternalAccounts["Downstream AWS Accounts\n(Governed Access)"]
    CodeArtifact["AWS CodeArtifact\n(Internal Package Registry)"]
    Dagster["Dagster\n(Orchestration — shared K8s)"]

    Platform["Datalake Pipeline Platform\n(data-infra + core-services + terraform-live)"]

    RabbitMQ -->|"Real-time clinical events (AMQPS)"| Platform
    DynamoDB -->|"CDC streams + point-in-time exports"| Platform
    EventBridge -->|"Scheduled triggers (SQS)"| Platform
    GitHub -->|"Manual ops triggers + CI/CD"| Platform
    GitHub -->|"API calls (dbt + aws_ops)"| Dagster
    Dagster -->|"dbt + aws_ops (per-slice agents)"| Platform
    CodeArtifact -->|"Internal Python packages"| Platform
    Platform -->|"PHI text for entity detection"| Comprehend
    Platform -->|"No-PHI Parquet + DBT tables"| Consumers
    Platform -->|"Governed lake access"| ExternalAccounts
    Operators -->|"Manual triggers via GitHub Actions"| GitHub
```

</Transform>

---
layout: default
class: text-sm
---

## External Dependencies

| Dependency | Role | Criticality | Fallback |
|-----------|------|-------------|---------|
| **RabbitMQ** | Source of all real-time clinical events | 🔴 CRITICAL | Messages queue in RabbitMQ; DESA resumes on recovery |
| **AWS SQS** | Inter-service coordination | 🔴 CRITICAL | Managed service; messages persist through failures |
| **Amazon Comprehend Medical** | Free-text PHI detection (TEDI) | 🟠 HIGH | Cross-region fallback (Frankfurt → eu-west-1) |
| **AWS Kinesis Firehose** | Batch delivery to S3 | 🟠 HIGH | Built-in retry; extended outage → event loss |
| **AWS Athena / Glue** | SQL engine for DBT + DDB replication | 🟠 HIGH | No in-platform fallback; processing pauses |
| **Dagster** | dbt + aws_ops orchestration | 🟠 HIGH | EventBridge → SQS → DRT fallback (disabled, not deleted) |
| **AWS EventBridge** | Scheduled triggers (data swamp + DDB CDC) | 🟡 MEDIUM | Manual GitHub Actions trigger available |
| **AWS CodeArtifact** | Internal Python packages | 🟡 MEDIUM | Service builds fail; no runtime impact |
