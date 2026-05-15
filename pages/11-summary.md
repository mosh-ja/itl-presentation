---
layout: section
---

# Tech Stack & Wrap-up

---
layout: default
class: text-sm
---

## Tech Stack Summary

<div class="grid grid-cols-2 gap-6">
<div>

| Category | Technology |
|---------|-----------|
| **Language** | Python 3.11 |
| **Package manager** | uv |
| **Package registry** | AWS CodeArtifact (private) |
| **Internal framework** | common-modules ≥8.117 |
| **Container runtime** | AWS EKS |
| **Event bus** | RabbitMQ (AMQPS) |
| **Stream ingestion** | AWS Kinesis Firehose |
| **Async messaging** | AWS SQS |
| **Event routing** | AWS SNS (single topic + filters) |
| **Scheduling** | Dagster + AWS EventBridge |
| **PHI NLP** | Amazon Comprehend Medical |

</div>
<div>

| Category | Technology |
|---------|-----------|
| **Object storage** | AWS S3 |
| **Schema catalog** | AWS Glue Catalog |
| **SQL engine** | AWS Athena |
| **Table format** | Apache Iceberg (via dbt-athena) |
| **Columnar format** | Apache Parquet (pandas + PyArrow) |
| **Analytical modeling** | dbt-core ≥1.10, dbt-athena ≥1.9.4 |
| **Orchestration** | Dagster (open-source) + dagster-dbt |
| **Run storage** | PostgreSQL (RDS/Aurora) |
| **Access governance** | AWS Lake Formation |
| **IaC** | Terragrunt + Terraform |
| **CI/CD** | GitHub Actions |
| **Secrets** | AWS Secrets Manager |

</div>
</div>

---
layout: cover
---

<div style="display:flex; flex-direction:column; height:100%; justify-content:center; align-items:center; text-align:center;">

<img src="/logo-white.png" style="height:56px; margin-bottom:2rem; opacity:0.9;" />

<h1 style="font-size:3rem; font-weight:700; color:#f47721; margin:0 0 1rem;">
  Thank You
</h1>

<p style="color:#aaaaaa; font-size:1.1rem; margin:0;">
  Moshe Jacobson · Data Engineer
</p>

</div>
