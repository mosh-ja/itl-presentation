---
layout: section
---

# Information Viewpoint

---
layout: default
class: text-sm
---

## Data Layers

| Layer | S3 Bucket Family | Format | PHI Status |
|-------|-----------------|--------|------------|
| **Raw** | `datalake-raw` | DESA envelope JSON | 🔴 PHI |
| **Silver** | `datalake-silver` | Canonical `GenericEvent` JSON | 🔴 PHI |
| **nSilver** | `datalake-nsilver` | De-identified `GenericEvent` JSON | 🟢 No-PHI |
| **Gold** | `datalake-gold` | Parquet (partitioned) | 🔴 PHI |
| **nGold** | `datalake-ngold` | Parquet (partitioned) | 🟢 No-PHI |
| **DBT Layer** | `datalake-dbt` | Iceberg + Parquet (Athena-managed) | 🟢 No-PHI |
| **DDB Raw** | `datalake-ddb*` | DynamoDB export JSON / CDC records | 🟡 Mixed |
| **DDB Lake** | Glue Iceberg tables | Iceberg + Parquet | 🟡 Mixed |
| **Data Swamp** | `datalake-data-swamp` | Ad-hoc Athena query output | 🟢 No-PHI |

---
layout: default
---

## PHI Boundary Enforcement

<div class="grid grid-cols-2 gap-8">
<div>

### PHI Split Mechanism

| Transition | PHI Path | No-PHI Path | Enforced By |
|-----------|----------|-------------|-------------|
| Raw → Standardized | S3 silver | S3 nsilver (via TEDI) | SNS subscription filters |
| Standardized → Columnar | S3 gold | S3 ngold | `S3ParquetWriter` bucket mapping |
| Columnar → Analytical | ❌ not exposed | nGold → Glue → Athena | Lake Formation grants |

</div>
<div>

### DBT Model Layers

| Layer | Prefix | Materialization |
|-------|--------|----------------|
| `0_utils` | — | Macros / seeds |
| `1_landing` | `lnd_` | View |
| `2_staging` | `stg_` | View |
| `3_serving` | `srv_` | Iceberg incremental |
| `4_use_cases` | `uc_` | Iceberg incremental |

> All `3_serving` + `4_use_cases` models tagged `PHI: "false"` in Lake Formation.

</div>
</div>
