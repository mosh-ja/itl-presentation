---
layout: section
---

# Development Viewpoint

---
layout: default
---

## Repository Organization

| Repository | Contents | Release Model |
|-----------|----------|--------------|
| `core-services` | DESA + 20+ other platform services | Independent per-service semver releases |
| `data-infra` | 5 pipeline microservices + DBT project + `dagster/` code location + Glue migration scripts | Independent per-service semver releases |
| `terraform-live` | All live AWS infrastructure as Terragrunt | PR-based apply; manual approval for prod |

<br>

### Standardized Service Structure

```
service_name/
├── src/service_name/
│   ├── main.py                 # asyncio entry; core_initializer
│   ├── common/dep_consts.py    # DI container IDs
│   ├── dtos/                   # Pydantic SQS event DTOs
│   └── event_handlers/         # SqsMessageHandlerInterface implementations
├── infra/manifest.yaml         # EKS: task count, memory, secrets
├── pyproject.toml              # uv-managed; private CodeArtifact index
└── tests/
```

---
layout: default
class: text-sm
---

## CI/CD Workflows

| Workflow | Trigger | Purpose |
|---------|---------|---------|
| Test + Build | Pull Request | Lint, test, Docker build per changed service |
| Auto Release | Merge to main | Semver bump + release |
| QA Deploy | Auto post-release | Deploy to `qa-simulator-1` |
| Prod Deploy | Manual approval | Deploy to `prod-ohio-1`, `prod-upmc-1`, `prod-frankfurt-1` |
| `trigger_dbt.yml` | `workflow_dispatch` | Manual dbt run — calls Dagster REST API |
| `trigger_aws_ops.yml` | `workflow_dispatch` | Manual aws_ops run — calls Dagster REST API |
| `trigger_data_swamp.yml` | `workflow_dispatch` | Manual data swamp run (SQS → DRT) |
| `call_ddb_replicator_*.yml` | `workflow_dispatch` | Manual CDC/INIT DDB replication |
| `export_ddb_table_to_s3.yml` | `workflow_dispatch` | DynamoDB point-in-time export + Glue partition |

<br>

> **Module dependencies:** All SQS services depend on `common-modules ≥8.117` (SQS, DI, logging, metrics). DRT and DDB Replicator additionally depend on `athena-client ≥2.0`. Both are from AWS CodeArtifact (private registry).
