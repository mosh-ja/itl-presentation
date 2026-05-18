---
layout: section
---

# Wrap-up

---
layout: default
---

## Key Decisions

* Producers declare PHI — TEDI only executes

* AWS services (Athena, Glue Jobs, DMS, etc.) do the heavy lifting

* Prefer semi-automation over full automation
  * Cost and complexity grow non-linearly

* Idempotent, loosely coupled services and models
  * Async components that can be retried safely

---
layout: default
---

## Tech Stack

<div class="grid grid-cols-3 gap-8 text-sm">
<div>

**Dev**
* Python · uv · Pydantic
* dbt · Dagster
* Apache Iceberg · Parquet

</div>
<div>

**AWS**
* SQS · SNS · EventBridge
* Kinesis Stream · Firehose
* S3 · Glue · Athena
* Comprehend Medical
* EKS
* RDS Postgres · DynamoDB
* IAM · Lake Formation
* CodeArtifact · Secrets Manager

</div>
<div>

**Ops**
* Docker
* GitHub Actions · Port
* Terraform · Terragrunt
* Coralogix · Grafana

</div>
</div>

---
layout: cover
---

<div style="display:flex; flex-direction:column; height:100%; justify-content:center; align-items:center; text-align:center;">

<h1 style="font-size:3rem; font-weight:700; color:#f47721; margin:0 0 1rem;">
  Thank You
</h1>

<p style="color:#aaaaaa; font-size:1.1rem; margin:0;">
  Moshe Jacobson
</p>

</div>
