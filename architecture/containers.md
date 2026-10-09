# Diagram 2: C4 Container

**Next.js decision:** we drew the Next.js app as **one** container (UI only); the business rules live in the separate API Application container.

```mermaid
---
config:
  layout: elk
title: "Diagram 2 - C4 Container: Allowance Tracker MVP (Peanut Baterbonia)"
---

flowchart TB
  KEY["<b>Key:</b> Dark blue box = person &nbsp;|&nbsp; Light blue box = container (separately deployable) &nbsp;|&nbsp; Grey box = external system &nbsp;|&nbsp; Arrow label = intent [protocol]"]:::keyn
  student["<b>Student</b><br/>[Person]"]:::person
  parent["<b>Parent / Guardian</b><br/>[Person]"]:::person
  subgraph BOUNDARY["Allowance Tracker [Software System boundary]"]
    web["<b>Web App</b><br/>[Container: Next.js, React, TypeScript]<br/>UI pages for logging expenses, viewing days left, and managing parent consent"]:::container
    api["<b>API Application</b><br/>[Container: Node.js, Express, TypeScript]<br/>Business rules: allowance, expenses, status, nudges, consent, weekly summaries"]:::container
    sched["<b>Scheduler</b><br/>[Container: Node.js, node-cron]<br/>Fires weekend-reminder and weekly-summary jobs on a timetable"]:::container
    db[("<b>Database</b><br/>[Container: PostgreSQL]<br/>Stores students, allowance periods, expenses, consent, summaries")]:::container
  end
  sms["<b>SMS Gateway</b><br/>[External System]"]:::ext
  msgr["<b>Messenger Platform</b><br/>[External System]"]:::ext
  student -->|"Logs expenses and views days left [HTTPS]"| web
  web -->|"Reads and writes allowance data [JSON over HTTPS, REST]"| api
  api -->|"Reads and writes records [SQL over TLS/TCP]"| db
  sched -->|"Triggers weekly-summary and reminder jobs [JSON over HTTPS, service token]"| api
  api -->|"Sends nudges and reminders [HTTPS REST]"| sms
  sms -->|"Delivers text to student [SMS]"| student
  api -->|"Sends weekly summary and invites [HTTPS REST]"| msgr
  msgr -->|"Reports invite acceptance [HTTPS webhook]"| api
  parent -->|"Accepts invite; reads summary [Messenger app]"| msgr

  classDef person fill:#08427b,stroke:#052e56,color:#fff;
  classDef system fill:#1168bd,stroke:#0b4884,color:#fff;
  classDef container fill:#438dd5,stroke:#2e6295,color:#fff;
  classDef ext fill:#999999,stroke:#6b6b6b,color:#fff;
  classDef actor fill:#fff3cd,stroke:#b58900,color:#000;
  classDef note fill:#ffffff,stroke:#999,stroke-dasharray:4 3,color:#000;
  classDef keyn fill:#fffbe6,stroke:#b58900,color:#000;
```

**Primary audience:** Developers and whoever deploys the system

**Risk it reduces:** Choosing the wrong technology split or leaving a protocol undefined.

**Owner / reviewer (proposed):** Erica G. Llena / Shamel O. Rebusora
