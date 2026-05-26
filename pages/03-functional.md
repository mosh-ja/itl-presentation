---
layout: section
---

# Functional Viewpoint

---
layout: default
---

## Backend Event Ingestion

<div class="mermaid-center">

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
    DET
    TEDI
    CM["AWS Comprehend Medical"]
    PHI_TB["PHI Table"]
    NOPHI_TB["No-PHI Table"]

    DESA --> DET
    DET --> TEDI
    TEDI <--> CM
    TEDI --> PHI_TB
    TEDI --> NOPHI_TB
```

</div>

<div style="margin-top: 0rem;">

* DESA - Listen to RabbitMQ
* DET - Events Transformation (normalizes different versions)
* TEDI - Text De-identification

</div>

---
layout: default
---

## CDC Ingestion

**Postgres CDC**

<div class="mermaid-center">

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
    CL_IN["Main Operational DB"]
    DMS["CDC Replication"]
    CDC_OUT["CDC Events Tables"]

    CL_IN --> DMS --> CDC_OUT
```

</div>

**DynamoDB CDC**

<div class="mermaid-center">

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
    DDB_IN["App State Store"]
    KS_Firehose["CDC Replication"]
    DDB_REP["Entity Materialization"]
    DDB_OUT["Mirrored Tables"]

    DDB_IN --> KS_Firehose --> DDB_REP --> DDB_OUT
```

</div>

<div style="margin-top: 1rem;">

* Entity Materialization - Calculate latest version

</div>

---
layout: default
---

## Transformation - Current

<div class="mermaid-center">

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
    CL["Main Operational DB"]
    Scheduler
    dbt["Data Transformation"]
    TB_OUT["Fact & Dim Tables"]
    Redash
    Tableau

    TB_IN --> dbt
    CL --> |"direct access"| dbt
    Scheduler -->|"launch"| dbt
    dbt --> TB_OUT
    TB_OUT --> Redash
    TB_OUT --> Tableau

    classDef bad stroke:#aa4444
    class Scheduler,dbt bad
```

</div>

<div style="margin-top: 1rem;">

* The scheduler runs all dbt models on every run
* The transformation evolved into a monolith
* Resulting in unnecessary compute and operational complexity.

</div>

---
layout: default
---

## Transformation - Solution

<div class="mermaid-center">

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
    TB_IN["Sources"]
    CP["Control Plane"]
    Agent["Scheduler"]

    dbt1["Data Transformation 1"]
    dbt2["Data Transformation 2"]
    dbtN["Data Transformation N"]
    TB_OUT["Fact & Dim Tables"]

    TB_IN --> dbt1
    TB_IN --> dbt2
    TB_IN --> dbtN
    CP --> Agent
    Agent --> dbt1
    Agent --> dbt2
    Agent --> dbtN
    dbt1 --> TB_OUT
    dbt2 --> TB_OUT
    dbtN --> TB_OUT

    classDef good stroke:#44aa44
    class CP,dbt1,dbt2,dbtN good
```

</div>

<div style="margin-top: 1rem;">

* Automatically manage dependencies
* Trigger execution based on data updates and schedules
* Break the monolithic transformation into modular pipelines

</div>
