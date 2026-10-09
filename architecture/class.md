# Diagram 6: Class

**Status fields and enumerations:** `AllowancePeriod.status` uses `AllowancePeriodStatus`; `ParentLink.consentStatus` uses `ConsentStatus`; `WeeklySummary.deliveryStatus` uses `DeliveryStatus`. Attribute types name their enumerations.

```mermaid
---
title: "Diagram 6 - Class: Domain model of the Allowance Tracker MVP"
---

classDiagram
  class Student {
    +UUID id
    +String fullName
    +String email
    +String phoneNumber
    +String passwordHash
    +DateTime createdAt
  }
  class ParentGuardian {
    +UUID id
    +String fullName
    +String messengerPsid
    +DateTime createdAt
  }
  class ParentLink {
    +UUID id
    +ConsentStatus consentStatus
    +DetailLevel detailLevel
    +DateTime consentedAt
    +DateTime revokedAt
  }
  class AllowancePeriod {
    +UUID id
    +Decimal startingAmount
    +Date startDate
    +Date nextPadalaDate
    +AllowancePeriodStatus status
  }
  class Expense {
    +UUID id
    +Decimal amount
    +ExpenseCategory category
    +Boolean isUnexpected
    +String note
    +DateTime spentAt
  }
  class NudgeSetting {
    +UUID id
    +Integer thresholdDays
    +Boolean smsEnabled
    +Boolean weekendReminderEnabled
  }
  class WeeklySummary {
    +UUID id
    +Date weekStart
    +Decimal totalSpent
    +Decimal unexpectedTotal
    +DeliveryStatus deliveryStatus
    +DateTime sentAt
  }
  class AllowancePeriodStatus {
    <<enumeration>>
    Active
    LowBalance
    Depleted
    Closed
  }
  class ConsentStatus {
    <<enumeration>>
    Pending
    Granted
    Revoked
  }
  class DeliveryStatus {
    <<enumeration>>
    Queued
    Sent
    Failed
  }
  class DetailLevel {
    <<enumeration>>
    TotalsOnly
    FullTransactions
  }
  class ExpenseCategory {
    <<enumeration>>
    Food
    Transport
    SchoolRequirement
    Social
    Emergency
    Other
  }
  Student "1" -- "0..1" NudgeSetting : configures
  Student "1" -- "0..*" AllowancePeriod : sets
  AllowancePeriod "1" -- "0..*" Expense : contains
  AllowancePeriod "1" -- "0..*" WeeklySummary : summarised by
  Student "1" -- "0..*" ParentLink : consents to
  ParentGuardian "1" -- "0..*" ParentLink : receives through
  ParentLink "1" -- "0..*" WeeklySummary : delivers
```

**Primary audience:** Developers

**Risk it reduces:** Inconsistent domain vocabulary and missing status enumerations.

**Owner / reviewer (proposed):** Clarence B. Ginete / Shamel O. Rebusora
