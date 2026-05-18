---
layout: section
---

# Perspectives

---
layout: default
---

## Security Perspective

<div class="grid grid-cols-2 gap-6">
<div>

| Concern | Implementation |
|---------|---------------|
| PHI/no-PHI separation | Parallel S3 buckets + Glue databases; Lake Formation grants restrict PHI access |
| Structured PHI de-identification | Deterministic hashing of declared `phi_paths` fields (ADR-015) |
| Free-text PHI de-identification | Comprehend Medical NLP detection + span masking (ADR-016) |
| SNS routing integrity | Message attribute filters enforce PHI vs no-PHI routing; misconfiguration = primary leakage risk |

</div>
<div>

| Concern | Implementation |
|---------|---------------|
| Service authentication | EKS IRSA — IAM roles attached to service accounts; no static credentials in containers |
| GitHub Actions auth | Static credentials + cross-account role assumption (ADR-008) |
| Dagster control plane | ALB-exposed; access restricted to internal networks and GHA IP ranges |
| Dagster API auth | Application-level API token stored as GHA secret; rotation procedure required |

</div>
</div>

---
layout: default
---

## Performance & Scalability Perspective

| Concern | Implementation |
|---------|---------------|
| Ingestion throughput | Firehose buffer settings bound raw object size; replica count multiplies consumer throughput |
| Events Transformation peak | 20 replicas in prod-ohio-1; independent SQS scaling |
| TEDI text-DI latency | Comprehend Medical API latency is the dominant bottleneck; no in-service caching |
| Relational Processor memory | Pandas in-process; mitigated by Firehose buffer sizing + replica scaling |
| Athena concurrency | Shared across Dagster agents (up to 5 slices), DRT, and DDB Replicator; workgroup quotas must be reviewed before enabling all 5 slice agents simultaneously |
| dbt partition isolation | `DailyPartitionsDefinition` — failure of one date does not block others |
| Scale knob | Replica count in `manifest.yaml` — static scaling (no HPA configured) |

---
layout: two-cols
---

## Location Perspective

| Concern | Implementation |
|---------|---------------|
| Multi-region deployment | 3 production regions: us-east-2 · eu-central-1 · eu-west-1 (QA) |
| Slice isolation | Each environment slice is fully independent within its account + region |
| Frankfurt Comprehend | TEDI calls Comprehend Medical in eu-west-1 from eu-central-1; accepted risk (ADR-017) |
| Data residency | Each slice's event data stays within its region (except the Frankfurt Comprehend call) |
| New region/slice | Add folder under `terraform-live/environments/aws/<account>/<region>/<slice>/` |

::right::

## Regulation Perspective

<div class="ml-4">

| Concern | Implementation |
|---------|---------------|
| HIPAA PHI handling | PHI and no-PHI paths physically separated at S3, Glue, and Lake Formation |
| Comprehend Medical HIPAA | AWS Comprehend Medical is HIPAA-eligible; covered by AWS BAA |
| PHI access control | Lake Formation governs access; analytics consumers only reach no-PHI tables |
| Audit trail | Raw events retained immutably in S3 for replay and audit |
| Frankfurt data residency | Cross-region PHI text transfer to eu-west-1 reviewed and accepted (ADR-017) |
| DBT PHI tagging | All `3_serving` and `4_use_cases` models tagged `PHI: "false"` in Lake Formation |

</div>
