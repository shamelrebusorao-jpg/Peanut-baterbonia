# Diagram 9: UML component

**Interfaces:** every external service (SMS Gateway, Messenger Send API) sits behind a port interface (`ISmsPort`, `IMessengerPort`) implemented by an adapter; services never call the vendors directly.

```mermaid
---
config:
  layout: elk
title: "Diagram 9 - UML Component: API Application container"
---
flowchart TB
  KEY["<b>Key:</b> Rectangle with «component» = component &nbsp;|&nbsp; Circle = interface &nbsp;|&nbsp; Solid line = provides the interface &nbsp;|&nbsp; Dashed arrow = requires the interface"]:::keyn
  web["Web App (container)"]:::extn
  sched["Scheduler (container)"]:::extn
  msgrp["Messenger Platform (external)"]:::ext
  subgraph APIC["«container» API Application"]
    direction TB
    iREST(("REST API"))
    routes["«component»<br/>Route Handlers"]
    iAuth(("IAuth"))
    iAllow(("IAllowance"))
    iExp(("IExpense"))
    iNudge(("INudge"))
    iCons(("IConsent"))
    iSumm(("ISummary"))
    auth["«component»<br/>Auth Service"]
    allow["«component»<br/>Allowance Service"]
    exp["«component»<br/>Expense Service"]
    nudge["«component»<br/>Nudge Service"]
    cons["«component»<br/>Consent Service"]
    summ["«component»<br/>Summary Service"]
    iRepo(("IRepositories"))
    iSms(("ISmsPort"))
    iMsg(("IMessengerPort"))
    repo["«component»<br/>Repositories"]
    smsad["«component»<br/>SMS Adapter"]
    msgad["«component»<br/>Messenger Adapter"]
  end
  db[("PostgreSQL")]:::ext
  smsg["SMS Gateway (external)"]:::ext
  msgrs["Messenger Send API (external)"]:::ext
  web -.-> iREST
  sched -.-> iREST
  msgrp -.->|"webhook"| iREST
  iREST --- routes
  routes -.-> iAuth
  routes -.-> iAllow
  routes -.-> iExp
  routes -.-> iNudge
  routes -.-> iCons
  routes -.-> iSumm
  iAuth --- auth
  iAllow --- allow
  iExp --- exp
  iNudge --- nudge
  iCons --- cons
  iSumm --- summ
  exp -.-> iAllow
  exp -.-> iNudge
  auth -.-> iRepo
  allow -.-> iRepo
  exp -.-> iRepo
  nudge -.-> iRepo
  cons -.-> iRepo
  summ -.-> iRepo
  nudge -.-> iSms
  cons -.-> iMsg
  summ -.-> iMsg
  iRepo --- repo
  iSms --- smsad
  iMsg --- msgad
  repo -.->|"SQL over TLS"| db
  smsad -.->|"HTTPS REST"| smsg
  msgad -.->|"HTTPS REST"| msgrs
  classDef ext fill:#999999,stroke:#6b6b6b,color:#fff;
  classDef extn fill:#438dd5,stroke:#2e6295,color:#fff;
  classDef keyn fill:#fffbe6,stroke:#b58900,color:#000;
```

**Primary audience:** Back-end developers

**Risk it reduces:** Direct coupling to SMS, Messenger, or the database that is hard to replace or test.

**Owner / reviewer (proposed):** Reese Goyal / Joram H. Hona
