---
layout: section
---

# Functional Viewpoint

---
layout: default
---

## Backend Event Ingestion

```mermaid
%%{init: {
  'theme': 'base',
  'themeVariables': {
    'background': '#0B0F14',
    'primaryColor': '#1e2a38',
    'primaryBorderColor': '#3a5068',
    'primaryTextColor': '#dce6f0',
    'lineColor': '#dce6f0',
    'edgeColor': '#7ab8e0',
    'edgeLabelBackground': '#1e2a38'
  }
}}%%
graph LR
    DESA
    Firehose
    DET
    TEDI
    CM["AWS Comprehend Medical"]
    PHI_TB["PHI Table"]
    NOPHI_TB["No-PHI Table"]

    DESA --> Firehose
    Firehose --> |"s3->sqs"| DET
    DET --> |"sns->sqs"| TEDI
    TEDI <--> CM
    TEDI --> PHI_TB
    TEDI --> NOPHI_TB
```

<div style="margin-top: 1rem;">

* DESA - Listen to RabbitMQ.
* DET - Events Transformation (normalizes different versions).
* TEDI - Text De-identification.

</div>

---
layout: default
---

## CDC Ingestion

**DynamoDB CDC**

```mermaid
%%{init: {
  'theme': 'base',
  'themeVariables': {
    'background': '#0B0F14',
    'primaryColor': '#1e2a38',
    'primaryBorderColor': '#3a5068',
    'primaryTextColor': '#dce6f0',
    'lineColor': '#dce6f0',
    'edgeColor': '#7ab8e0',
    'edgeLabelBackground': '#1e2a38'
  }
}}%%
graph LR
    DDB_IN["DynamoDB Tables"]
    KS["Kinesis Stream"]
    Firehose
    DDB_REP["DDB Replicator"]
    DDB_OUT["Mirrored Tables"]

    DDB_IN --> KS --> Firehose --> DDB_REP --> DDB_OUT
```

**Clinium CDC**

```mermaid
%%{init: {
  'theme': 'base',
  'themeVariables': {
    'background': '#0B0F14',
    'primaryColor': '#1e2a38',
    'primaryBorderColor': '#3a5068',
    'primaryTextColor': '#dce6f0',
    'lineColor': '#dce6f0',
    'edgeColor': '#7ab8e0',
    'edgeLabelBackground': '#1e2a38'
  }
}}%%
graph LR
    CL_IN["Clinium DB"]
    DMS["DMS"]
    CDC_OUT["CDC Events Tables"]

    CL_IN --> DMS --> CDC_OUT
```

<div style="margin-top: 1rem;">

* Kinesis Stream, Firehose, and DMS are AWS services.
* Clinium DB — RDS (Postgres), the main operational database.

</div>

---
layout: default
---

## Transformation

```mermaid
%%{init: {
  'theme': 'base',
  'themeVariables': {
    'background': '#0B0F14',
    'primaryColor': '#1e2a38',
    'primaryBorderColor': '#3a5068',
    'primaryTextColor': '#dce6f0',
    'lineColor': '#dce6f0',
    'edgeColor': '#7ab8e0',
    'edgeLabelBackground': '#1e2a38'
  }
}}%%
graph LR
    TB_IN["Ingestion Tables"]
    CL["Clinium DB"]
    CP["Dagster Control Plane"]
    Agent["Dagster Agent"]
    dbt
    TB_OUT["Fact & Dim Tables"]
    Redash
    Tableau

    TB_IN --> dbt
    CL --> |"Athena data source"| dbt
    CP -->|"launch"| Agent
    Agent --> dbt
    dbt --> TB_OUT
    TB_OUT --> Redash
    TB_OUT --> Tableau
```

<div style="margin-top: 2rem;">

* Dagster Control Plane is cross-region and cross-environment.
* Dagster Agent is per environment.
* All transformations are done through Athena.
* Permissions are managed by IAM + Lake Formation.

</div>
