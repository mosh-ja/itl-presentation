---
layout: section
---

# Perspectives

---
layout: default
---

## Availability & Resilience

* Critical buckets replicated across regions

* Each service runs ≥ 2 pods on separate nodes

* Cross-region Comprehend Medical where unavailable locally

* DBT snapshots retained 2 weeks; deleted files +2 weeks via S3 lifecycle

---
layout: default
---

## Regulation & Security

* Non-DBT buckets enforce 1-month retention

* Account decommissioning via Glue-based churn process

* Access managed via IAM and Lake Formation

* Lake Formation enforces column-level PHI access control

* PHI columns use `_phi` suffix; DI equivalents use `_di`

* All S3 access audited (small files incur significant cost overhead)
