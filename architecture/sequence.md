# Diagram 5: Sequence

**Why this flow:** it changes a status and calls an external system (SMS Gateway), and it depends on the riskiest assumption in our Javelin board (students logging expenses on the day they happen).

```mermaid
---
title: "Diagram 5 - Sequence: Record an expense, update status, and send a low-balance nudge"
---
sequenceDiagram
  autonumber
  actor S as Student
  participant P as Record Expense page<br/>(Web App)
  participant A as API Application
  participant D as PostgreSQL<br/>(Database)
  participant M as SMS Gateway<br/>(external)
  S->>P: Enter amount and category, tick "biglaang gastos" if unexpected, submit
  P->>A: POST /expenses {amount, category, isUnexpected} (HTTPS, session token)
  A->>A: Validate session, input, and that an Active or LowBalance period exists
  alt Invalid session, invalid input, or no open period
    A-->>P: 401 / 422 / 409 error
    P-->>S: Show error, nothing is saved
  else Valid request
    A->>D: INSERT expense into the open allowance period
    D-->>A: Expense saved
    A->>D: SELECT remaining balance, next padala date, nudge setting
    D-->>A: Balance, thresholdDays, smsEnabled
    A->>A: Compute daysLeft and the new period status
    opt Status changed to LowBalance or Depleted
      A->>D: UPDATE allowance_period SET status
      D-->>A: Status updated
      opt smsEnabled is true
        A-)M: sendSms(student phone, low-balance nudge) [asynchronous, HTTPS]
        Note right of M: Failure is logged and never blocks the response
      end
    end
    A-->>P: 201 {remaining, daysLeft, status}
    P-->>S: Show updated days left and status
  end
```

**Primary audience:** Front-end and back-end developers

**Risk it reduces:** Wrong balance or status after an expense, or an SMS failure that silently breaks saving.

**Owner / reviewer (proposed):** Shamel O. Rebusora / Reese Goyal
