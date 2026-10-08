# Diagram 3: Use case

**Traceability to the MVP:** UC2 = log starting weekly allowance; UC3 + UC4 = quick flag for a *biglaang gastos*; UC5 + UC6 + UC7 = running "days left before the next padala" with a low-balance nudge; UC8 to UC10 = weekly summary for the parent (Experiment 2). UC1 supports all student goals.

```mermaid
---
config:
  layout: elk
title: "Diagram 3 - Use Case: Allowance Tracker MVP (Peanut Baterbonia)"
---
flowchart LR
  KEY["<b>Key:</b> Yellow oval = primary actor &nbsp;|&nbsp; Grey oval = external system actor &nbsp;|&nbsp; Purple-outlined oval = use case (verb + object) &nbsp;|&nbsp; Solid line = association &nbsp;|&nbsp; Dashed arrow = extend"]:::keyn
  student(["Student"]):::actor
  parent(["Parent / Guardian"]):::actor
  sms(["SMS Gateway<br/>(external system)"]):::ext
  msgr(["Messenger Platform<br/>(external system)"]):::ext
  subgraph SYS["Allowance Tracker (system boundary)"]
    UC1(["UC1 Log in"])
    UC2(["UC2 Set weekly allowance"])
    UC3(["UC3 Record expense"])
    UC4(["UC4 Flag unexpected expense"])
    UC5(["UC5 View days left"])
    UC6(["UC6 Set low-balance nudge"])
    UC7(["UC7 Receive low-balance nudge"])
    UC8(["UC8 Grant parent consent"])
    UC9(["UC9 Accept summary invite"])
    UC10(["UC10 Read weekly summary"])
  end
  student --- UC1
  student --- UC2
  student --- UC3
  student --- UC5
  student --- UC6
  student --- UC7
  student --- UC8
  UC4 -.->|"«extend»"| UC3
  UC7 --- sms
  parent --- UC9
  parent --- UC10
  UC9 --- msgr
  UC10 --- msgr

  classDef person fill:#08427b,stroke:#052e56,color:#fff;
  classDef system fill:#1168bd,stroke:#0b4884,color:#fff;
  classDef container fill:#438dd5,stroke:#2e6295,color:#fff;
  classDef ext fill:#999999,stroke:#6b6b6b,color:#fff;
  classDef actor fill:#fff3cd,stroke:#b58900,color:#000;
  classDef note fill:#ffffff,stroke:#999,stroke-dasharray:4 3,color:#000;
  classDef keyn fill:#fffbe6,stroke:#b58900,color:#000;
```

**Primary audience:** Team and instructor (scope check)

**Risk it reduces:** Building features that are not in the validated MVP list (scope creep).

**Owner / reviewer (proposed):** Joram H. Hona / Erica G. Llena
