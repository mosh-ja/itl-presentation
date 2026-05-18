---
layout: section
---

# Development Viewpoint

---
layout: default
class: text-sm
---

## Repositories

- **`data-infra`**
  - Services + dbt + Dagster code location
  - Each service has its own pyproject.toml and Dockerfile
  - GitHub Actions are used as an ops tool (`trigger_ddb_replicator`)

- **`terraform-live`**
  - One folder per env slice
  - Use internal terraform-modules for reuse
