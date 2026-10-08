# Diagram 4: Activity

**Core workflow:** the weekly allowance cycle, from setting the allowance to the parent summary. Lanes: Student, System, Parent / Guardian.

```mermaid
---
title: "Diagram 4 - Activity Diagram: Weekly allowance cycle, from setting the allowance to the parent summary"
---
flowchart TB
  KEY["<b>Key:</b> Black circle = start &nbsp;|&nbsp; Ringed circle = end &nbsp;|&nbsp; Rectangle = action &nbsp;|&nbsp; Diamond = decision, edges carry [guards] &nbsp;|&nbsp; Each titled box (Student, System, Parent / Guardian) = one swimlane"]:::keyn
  subgraph LS["Student"]
    direction TB
    S0(("Start")):::startn
    S1["Set weekly allowance and next padala date"]
    S3["Enter expense (amount, category)"]
    S4{"Unexpected expense?"}
    S4a["Tick the biglaang gastos flag"]
    S5["Read nudge and adjust spending"]
    S6{"Another expense this week?"}
  end
  subgraph LY["System"]
    direction TB
    Y1["Create allowance period (status Active)"]
    Y2["Save expense"]
    Y3["Recalculate remaining balance and days left"]
    Y4{"Low or zero balance?"}
    Y5["Set status LowBalance or Depleted; send SMS nudge"]
    Y6["Show updated days left"]
    Y7["Period ends: set status Closed"]
    Y8{"Parent consent Granted?"}
    Y9["Build weekly summary at the agreed detail level"]
    Y10["Send summary through Messenger"]
    Y11["Skip summary"]
    E2(("End")):::endn
  end
  subgraph LP["Parent / Guardian"]
    direction TB
    P1["Read weekly summary"]
    E1(("End")):::endn
  end
  S0 --> S1 --> Y1 --> S3 --> S4
  S4 -->|"[yes]"| S4a --> Y2
  S4 -->|"[no]"| Y2
  Y2 --> Y3 --> Y4
  Y4 -->|"[yes]"| Y5 --> S5 --> S6
  Y4 -->|"[no]"| Y6 --> S6
  S6 -->|"[yes]"| S3
  S6 -->|"[no: period over]"| Y7 --> Y8
  Y8 -->|"[yes]"| Y9 --> Y10 --> P1 --> E1
  Y8 -->|"[no]"| Y11 --> E2
  classDef startn fill:#000,stroke:#000,color:#fff;
  classDef endn fill:#fff,stroke:#000,stroke-width:4px,color:#000;
  classDef keyn fill:#fffbe6,stroke:#b58900,color:#000;
```

**Primary audience:** Team and testers

**Risk it reduces:** Gaps in the workflow, e.g. what happens when the parent has not given consent.

**Owner / reviewer (proposed):** Joram H. Hona / Clarence B. Ginete
