# Diagram 8: Package

**Layering rule:** Routes call services only, services depend on domain interfaces (ports) and never import adapters or the database client, and the web app and scheduler reach the API only over HTTPS, never by importing `apps/api` code.

```text
apps/
  web/        app/  components/  lib/api-client
  scheduler/  src/jobs/
  api/        src/server.ts  src/routes  src/services  src/repositories
              src/adapters   src/domain  src/db
docs/architecture/
```

```mermaid
---
config:
  layout: elk
title: "Diagram 8 - Package: Planned folder structure and allowed dependencies"
---
flowchart TB
  KEY["<b>Key:</b> Box = folder (package) &nbsp;|&nbsp; Solid arrow = may import &nbsp;|&nbsp; Dashed arrow = runtime call over HTTPS (no import)"]:::keyn
  subgraph WEB["apps/web (Next.js)"]
    direction TB
    wapp["app/<br/>pages and layouts"]
    wcomp["components/<br/>UI pieces"]
    wlib["lib/api-client<br/>typed HTTPS calls"]
  end
  subgraph SCH["apps/scheduler (Node.js)"]
    direction TB
    jobs["src/jobs/<br/>weekly-summary, weekend-reminder"]
  end
  subgraph API["apps/api (Node.js)"]
    direction TB
    server["src/server.ts<br/>composition root"]
    routes["src/routes/<br/>HTTP handlers"]
    services["src/services/<br/>business rules"]
    repos["src/repositories/<br/>data access"]
    adapters["src/adapters/<br/>sms, messenger"]
    domain["src/domain/<br/>entities, enums, ports"]
    dbp["src/db/<br/>client, migrations"]
  end
  wapp --> wcomp
  wapp --> wlib
  wcomp --> wlib
  wlib -.->|"HTTPS call, not an import"| routes
  jobs -.->|"HTTPS call, not an import"| routes
  server --> routes
  server --> services
  server --> repos
  server --> adapters
  routes --> services
  services --> domain
  repos --> domain
  repos --> dbp
  adapters --> domain

  classDef person fill:#08427b,stroke:#052e56,color:#fff;
  classDef system fill:#1168bd,stroke:#0b4884,color:#fff;
  classDef container fill:#438dd5,stroke:#2e6295,color:#fff;
  classDef ext fill:#999999,stroke:#6b6b6b,color:#fff;
  classDef actor fill:#fff3cd,stroke:#b58900,color:#000;
  classDef note fill:#ffffff,stroke:#999,stroke-dasharray:4 3,color:#000;
  classDef keyn fill:#fffbe6,stroke:#b58900,color:#000;
```

**Primary audience:** Developers

**Risk it reduces:** Tangled imports that make the code hard to change or test.

**Owner / reviewer (proposed):** Shamel O. Rebusora / Clarence B. Ginete
