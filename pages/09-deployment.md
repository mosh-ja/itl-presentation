---
layout: section
---

# Deployment Viewpoint

---
layout: default
---

## AWS Account Topology & Environment Slices

<div class="grid grid-cols-2 gap-8">
<div>

### AWS Accounts

| Account Band | Role |
|-------------|------|
| `aws-prod-processing` | Production pipeline runtime |
| `aws-qa-processing` | QA pipeline runtime; isolated from prod |
| `aws-prod-commercial` | Commercial DynamoDB source tables |
| `aws-master` / `aws-mgmt` | Organization management, SSO |

</div>
<div>

### Environment Slices

| Slice | Account | Region | Type |
|-------|---------|--------|------|
| `prod-ohio-1` | aws-prod-processing | us-east-2 | Production |
| `prod-upmc-1` | aws-prod-processing | us-east-2 | Production |
| `prod-frankfurt-1` | aws-prod-processing | eu-central-1 | Production (EU) |
| `qa-simulator-1` | aws-qa-processing | eu-west-1 | QA (primary) |
| `qa-solaris-1` | aws-qa-processing | eu-west-1 | QA (secondary) |

</div>
</div>

---
layout: default
---

## Infrastructure per Slice

<Transform :scale="0.72">

```mermaid
graph TD
    subgraph "Processing Account / Region"
        subgraph "EKS Cluster"
            Pods["Service Pods\nDESA · ET · TEDI · RP · DRT · DDBR · Dagster Agent"]
        end
        subgraph "Messaging"
            Firehose["Kinesis Firehose"]
            SNS_d["SNS: datalake-processing-sns"]
            SQS_d["SQS queues (one per service)"]
            EB_d["EventBridge (data-swamp + DDB CDC)"]
        end
        subgraph "Storage"
            S3_PHI["S3 PHI buckets\n(raw, silver, gold)"]
            S3_noPHI["S3 no-PHI buckets\n(nsilver, ngold, dbt, data-swamp)"]
        end
        subgraph "Analytical Layer"
            Glue_d["AWS Glue Catalog"]
            Athena_d["AWS Athena"]
            LF_d["Lake Formation\n(access governance)"]
        end
    end

    Pods --> Firehose
    Pods --> SNS_d
    Pods --> SQS_d
    Pods --> Athena_d
    Firehose --> S3_PHI
    Firehose --> S3_noPHI
    S3_PHI --> Glue_d
    S3_noPHI --> Glue_d
    Athena_d --> Glue_d
    LF_d -.->|"governs"| Glue_d
    EB_d --> SQS_d
```

</Transform>

---
layout: default
---

## Dagster Cross-Slice Topology

<Transform :scale="0.62">

```mermaid
graph TD
    subgraph Shared["Shared K8s Account (Control Plane)"]
        Web["Dagster Webserver\n(UI + REST API)"]
        Daemon["Dagster Daemon\n(scheduler + run launcher)"]
        PG[("PostgreSQL\nrun storage")]
        ALB["ALB (ingress)"]
        ALB --> Web
        Web --- Daemon
        Web --- PG
        Daemon --- PG
    end

    GHA["GitHub Actions\n(trigger_dbt, trigger_aws_ops)"]
    GHA -->|"POST /runs (API token)"| ALB

    subgraph Ohio["aws-prod-processing / us-east-2"]
        AgentOhio["Dagster Agent\n(prod-ohio-1)"]
        AthenaOhio["Athena us-east-2"]
        AgentOhio --> AthenaOhio
    end
    subgraph Frankfurt["aws-prod-processing / eu-central-1"]
        AgentFrank["Dagster Agent\n(prod-frankfurt-1)"]
        AthenaFrank["Athena eu-central-1"]
        AgentFrank --> AthenaFrank
    end
    subgraph QASim["aws-qa-processing / eu-west-1"]
        AgentSim["Dagster Agent\n(qa-simulator-1)"]
        AthenaSim["Athena eu-west-1"]
        AgentSim --> AthenaSim
    end

    AgentOhio -.->|"outbound HTTPS/gRPC poll"| Daemon
    AgentFrank -.->|"outbound HTTPS/gRPC poll"| Daemon
    AgentSim -.->|"outbound HTTPS/gRPC poll"| Daemon
```

</Transform>

> Agents in processing accounts poll the control plane **outbound** — no inbound connections, no VPC peering required.
