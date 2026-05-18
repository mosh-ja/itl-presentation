---
layout: section
---

# Deployment Viewpoint

---
layout: default
---

## Deployment Flow

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
    Push["Push to master\n(data-infra)"]
    GHA1["platform-workflow\nGitHub Action"]
    CI["Tests · Lint · Build"]
    Fail["Failed"]
    Pass["Passed"]
    SlackFail["Slack\n(error details)"]
    SlackPass["Slack\n(deploy link)"]
    Approve["Team approves"]
    GHA2["Deploy\nGitHub Action"]
    Prod["Production\n(K8s per env)"]

    Push --> GHA1 --> CI
    CI --> Fail
    CI --> Pass
    Fail --> SlackFail
    Pass --> SlackPass --> Approve --> GHA2 --> Prod

    classDef ok fill:#1a4a1a,stroke:#44aa44,color:#ffffff
    classDef bad fill:#4a1a1a,stroke:#aa4444,color:#ffffff

    class Pass,SlackPass,Approve ok
    class Fail,SlackFail bad
```

---
layout: default
---

## Dagster Topology

<Transform :scale="0.75">

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
    'edgeLabelBackground': '#1e2a38',
    'clusterBkg': '#0a1825',
    'clusterBorder': '#00aaff'
  }
}}%%
graph TD
    subgraph CP["Global K8s — deployed once"]
        Web["Dagster Webserver\n(UI + REST API)"]
        Daemon["Dagster Daemon\n(scheduler)"]
        PG[("PostgreSQL\n(run storage)")]
        Web --- Daemon
        Web --- PG
        Daemon --- PG
    end
    subgraph EK["Env K8s — one per env"]
        Agent["Dagster Agent"]
        Athena["Athena\n(per region)"]
        Agent --> Athena
    end
    Agent -.->|"outbound poll"| Daemon
```

- Agents run with pipeline services using outbound-only polling

</Transform>
