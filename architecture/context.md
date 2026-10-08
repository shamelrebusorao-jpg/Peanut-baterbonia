# Diagram 1: C4 System Context

**System description:** the Allowance Tracker is drawn as one box; every arrow states what the sender is trying to do.

```mermaid
---
config:
  layout: elk
title: "Diagram 1 - C4 System Context: Allowance Tracker MVP (Peanut Baterbonia)"
---
flowchart TB
  KEY["<b>Key:</b> Dark blue box = person &nbsp;|&nbsp; Mid blue box = software system in scope &nbsp;|&nbsp; Grey box = external system &nbsp;|&nbsp; Arrow label = what the sender is trying to do"]:::keyn
  student["<b>Student</b><br/>[Person]<br/>4th-year BSIS boarding-house student who manages a fixed weekly allowance"]:::person
  parent["<b>Parent / Guardian</b><br/>[Person]<br/>Sends the weekly allowance (padala) and wants to know where it went"]:::person
  sys["<b>Allowance Tracker</b><br/>[Software System]<br/>Lets a student log a weekly allowance and expenses, see days left before the next padala, and share a weekly summary with a parent"]:::system
  sms["<b>SMS Gateway</b><br/>[External System]<br/>Delivers text-message nudges and reminders to the student's phone"]:::ext
  msgr["<b>Messenger Platform</b><br/>[External System]<br/>Delivers messages to the parent's Messenger account and reports replies"]:::ext
  student -->|"Logs allowance and expenses; checks days left"| sys
  sys -->|"Asks to text the student a low-balance nudge or weekend reminder"| sms
  sms -->|"Delivers the nudge or reminder (SMS)"| student
  sys -->|"Asks to send the weekly summary or summary invite to the parent"| msgr
  msgr -->|"Reports that the parent accepted the invite"| sys
  parent -->|"Accepts the summary invite; reads the weekly summary"| msgr

  classDef person fill:#08427b,stroke:#052e56,color:#fff;
  classDef system fill:#1168bd,stroke:#0b4884,color:#fff;
  classDef container fill:#438dd5,stroke:#2e6295,color:#fff;
  classDef ext fill:#999999,stroke:#6b6b6b,color:#fff;
  classDef actor fill:#fff3cd,stroke:#b58900,color:#000;
  classDef note fill:#ffffff,stroke:#999,stroke-dasharray:4 3,color:#000;
  classDef keyn fill:#fffbe6,stroke:#b58900,color:#000;
```

**Primary audience:** Instructor, team, and non-technical partners (e.g. parents)

**Risk it reduces:** A missed user role or external system, so an integration surprises us late.

**Owner / reviewer (proposed):** Erica G. Llena / Joram H. Hona
