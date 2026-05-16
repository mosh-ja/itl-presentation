---
layout: section
---

# Context Viewpoint

---
layout: default
---

## Context Diagram

<div style="width:100%; text-align:center; transform:scale(0.85); transform-origin:top center;">

```mermaid
%%{init: {
  'theme': 'base',
  'themeVariables': {
    'background': '#0B0F14',
    'primaryColor': '#1e2a38',
    'primaryBorderColor': '#3a5068',
    'primaryTextColor': '#dce6f0',
    'lineColor': '#dce6f0',
    'edgeColor': '#7ab8e0'
  }
}}%%
graph LR
    BE["Backend Events"]
    DDB["DynamoDB CDC"]
    PG["Postgres DB"]
    CM["Comprehend Medical"]
    Platform["Data Pipeline"]
    Tableau["Tableau"]
    Redash["Redash"]

    BE --> Platform
    DDB --> Platform
    PG --> Platform
    CM <--> Platform
    Platform --> Tableau
    Platform --> Redash
```

</div>

---
layout: default
class: text-sm
---

## Core Components

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
