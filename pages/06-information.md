---
layout: section
---

# Information Viewpoint

---
layout: default
class: text-sm
---

## Storage & Conventions

* dbt: S3 + Glue + Iceberg (Athena-managed)

* Non-dbt: S3 + Glue + Parquet / JSONL

* S3 naming convention: `viz-{env}-datalake-{name}-{account}-{region}`

* Glue DB naming convention: `datalake-{name}`

* Non-dbt S3 mainly partitioned by `year` / `month` / `day`

---
layout: default
---

## DBT

| Layer         | Prefix | Materialization  | Purpose |
| ------------- | ------ | ---------------- | ------- |
| `1_landing`   | `lnd_` | Mostly views     | Basic transformations |
| `2_staging`   | `stg_` | Mostly views     | Extract nested data, align schema |
| `3_serving`   | `srv_` | Mostly tables    | Source of truth — wide denormalized facts |
| `4_use_cases` | `uc_`  | Tables and views | Ready to use for specific use cases |

* All tables are in incremental strategy

* dbt tables carry `dbt_effective_date` or `dbt_exe_ts`
