---
layout: section
---

# Deployment Viewpoint

---
layout: default
class: dagster-topology
---

## Dagster Topology

<style>
.slidev-layout.dagster-topology .mermaid {
  display: flex;
  justify-content: center;
  width: 100%;
}
.slidev-layout.dagster-topology .mermaid svg {
  width: 85% !important;
  max-width: 62rem;
  height: auto !important;
}
.slidev-layout.dagster-topology h2,
.slidev-layout.dagster-topology > p {
  text-align: left;
}
</style>

```mermaid
%%{init: {
  'theme': 'base',
  'flowchart': { 'nodeSpacing': 28, 'rankSpacing': 35, 'padding': 10 },
  'themeVariables': {
    'background': '#0B0F14',
    'primaryColor': '#1e2a38',
    'primaryBorderColor': '#3a5068',
    'primaryTextColor': '#dce6f0',
    'lineColor': '#dce6f0',
    'edgeColor': '#7ab8e0',
    'edgeLabelBackground': '#1e2a38',
    'fontSize': '13px',
    'clusterBkg': '#0a1825',
    'clusterBorder': '#00aaff'
  }
}}%%
graph TD
    subgraph CP["Global K8s — once"]
        Web["Webserver"]
        Daemon["Daemon"]
        PG[("PostgreSQL")]
        Web --- Daemon
        Web --- PG
        Daemon --- PG
    end
    subgraph EK["Env K8s — per env"]
        Agent["Agent"]
        Athena["Athena"]
        Agent --> Athena
    end
    Agent -.->|poll| Daemon
```

* Agents run with pipeline services using outbound-only polling

---
layout: default
class: text-sm
---

## Deployment Flow

```mermaid
%%{init: {
  'theme': 'base',
  'flowchart': { 'nodeSpacing': 28, 'rankSpacing': 35, 'padding': 10 },
  'themeVariables': {
    'background': '#0B0F14',
    'primaryColor': '#1e2a38',
    'primaryBorderColor': '#3a5068',
    'primaryTextColor': '#dce6f0',
    'lineColor': '#dce6f0',
    'edgeColor': '#7ab8e0',
    'edgeLabelBackground': '#1e2a38',
    'fontSize': '13px',
    'clusterBkg': '#0a1825',
    'clusterBorder': '#3a5068'
  }
}}%%
graph LR
    subgraph build["Build"]
        direction LR
        Push["Push"] --> GHA["platform-workflow"] --> CI["Lint\nTest\n..."]
        CI --> Fail["Fail"]
        CI --> Pass["Pass"]
        Fail --> SlackFail["Slack"]
        Pass --> SlackPass["Slack"]
    end
    subgraph release["Release"]
        direction LR
        Approve["Approve"] --> Deploy["Deploy"] --> Prod["Prod K8s"]
    end
    SlackPass --> Approve

    classDef ok stroke:#44aa44
    classDef bad stroke:#aa4444

    class Pass,SlackPass,Approve,Deploy,Prod ok
    class Fail,SlackFail bad
```

<div style="margin-top: 1rem;">

* platform-workflow is the central CI/CD hub for all repos

* It enforces cross-repo quality gates via automated jobs

* Workflows integrate with Slack for visibility and control

</div>
