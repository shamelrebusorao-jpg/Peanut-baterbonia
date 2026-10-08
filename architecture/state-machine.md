# Diagram 7: State machine

**Main entity:** `AllowancePeriod`. The four state names are copied exactly into the `AllowancePeriodStatus` enumeration of Diagram 6: Active, LowBalance, Depleted, Closed.

```mermaid
---
title: "Diagram 7 - State machine: AllowancePeriod.status"
---
stateDiagram-v2
  [*] --> Active : allowanceSet
  Active --> LowBalance : expenseRecorded [daysLeft at or below thresholdDays]
  Active --> Depleted : expenseRecorded [remaining balance is zero]
  LowBalance --> Depleted : expenseRecorded [remaining balance is zero]
  Active --> Closed : periodEnded
  LowBalance --> Closed : periodEnded
  Depleted --> Closed : periodEnded
  Closed --> [*]
  note right of LowBalance
    SMS nudge is sent on entry
    when smsEnabled is true
  end note
```

**Primary audience:** Developers and testers

**Risk it reduces:** Illegal status changes of an allowance period (e.g. reopening a Closed period).

**Owner / reviewer (proposed):** Clarence B. Ginete / Reese Goyal
