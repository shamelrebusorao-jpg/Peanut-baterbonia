# Diagram 10: Deployment

**Provisional:** hosting providers are not chosen yet, so nodes use generic names. No secrets or real addresses are shown.

```mermaid
---
config:
  layout: elk
title: "Diagram 10 - Deployment (Provisional): Allowance Tracker MVP"
---
flowchart TB
  KEY["<b>Key:</b> Large box = «node» or «device» &nbsp;|&nbsp; Inner box = «execution environment» and its «artifact» &nbsp;|&nbsp; Arrow label = protocol &nbsp;|&nbsp; Provider names and addresses are not chosen yet &nbsp;|&nbsp; Secrets stay in the host's secret store"]:::keyn
  subgraph DEV1["«device» Student's phone or laptop"]
    direction TB
    br["«execution environment»<br/>Web browser<br/>«artifact» Web App pages (HTML, JS)"]
  end
  subgraph DEV2["«device» Parent's phone"]
    direction TB
    mapp["«execution environment»<br/>Messenger app"]
  end
  subgraph N1["«node» Web Hosting (provisional)"]
    direction TB
    e1["«execution environment»<br/>Node.js runtime<br/>«artifact» web-app build"]
  end
  subgraph N2["«node» Application Hosting (provisional)"]
    direction TB
    e2["«execution environment»<br/>Node.js runtime<br/>«artifact» api bundle"]
  end
  subgraph N3["«node» Job Hosting (provisional)"]
    direction TB
    e3["«execution environment»<br/>Node.js runtime<br/>«artifact» scheduler bundle"]
  end
  subgraph N4["«node» Managed Database (provisional)"]
    direction TB
    e4["«execution environment»<br/>PostgreSQL server<br/>«artifact» schema and migrations"]
  end
  xs["«external system»<br/>SMS Gateway"]:::ext
  xm["«external system»<br/>Messenger Platform"]:::ext
  br -->|"HTTPS (page delivery)"| e1
  br -->|"HTTPS, JSON REST"| e2
  e3 -->|"HTTPS, JSON REST"| e2
  e2 -->|"TLS over TCP (PostgreSQL protocol)"| e4
  e2 -->|"HTTPS REST"| xs
  e2 -->|"HTTPS REST"| xm
  xm -->|"HTTPS webhook"| e2
  xs -->|"SMS"| br
  xm -->|"Messenger protocol"| mapp

  classDef person fill:#08427b,stroke:#052e56,color:#fff;
  classDef system fill:#1168bd,stroke:#0b4884,color:#fff;
  classDef container fill:#438dd5,stroke:#2e6295,color:#fff;
  classDef ext fill:#999999,stroke:#6b6b6b,color:#fff;
  classDef actor fill:#fff3cd,stroke:#b58900,color:#000;
  classDef note fill:#ffffff,stroke:#999,stroke-dasharray:4 3,color:#000;
  classDef keyn fill:#fffbe6,stroke:#b58900,color:#000;
```

**Primary audience:** Whoever hosts the app, and the instructor

**Risk it reduces:** Hosting surprises: unplanned cost, missing nodes, or unprotected network paths.

**Owner / reviewer (proposed):** Erica G. Llena / Reese Goyal
