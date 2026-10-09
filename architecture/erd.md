# Diagram 11: ERD (draft)

**Rules:** one table per stored class (7); enumerations are stored as enum columns; PII columns are marked `PII`; `NUDGE_SETTING.student_id` is unique to give the 0..1 multiplicity of Diagram 6.

```mermaid
---
title: "Diagram 11 - ERD (draft): Allowance Tracker MVP"
---

erDiagram
  STUDENT ||--o{ ALLOWANCE_PERIOD : sets
  ALLOWANCE_PERIOD ||--o{ EXPENSE : contains
  STUDENT ||--o| NUDGE_SETTING : configures
  STUDENT ||--o{ PARENT_LINK : "consents to"
  PARENT_GUARDIAN ||--o{ PARENT_LINK : "receives through"
  PARENT_LINK ||--o{ WEEKLY_SUMMARY : delivers
  ALLOWANCE_PERIOD ||--o{ WEEKLY_SUMMARY : "summarised by"
  STUDENT {
    uuid id PK
    varchar full_name "PII"
    varchar email UK "PII"
    varchar phone_number "PII"
    varchar password_hash "PII credential"
    timestamp created_at
  }
  PARENT_GUARDIAN {
    uuid id PK
    varchar full_name "PII"
    varchar messenger_psid UK "PII"
    timestamp created_at
  }
  PARENT_LINK {
    uuid id PK
    uuid student_id FK
    uuid parent_guardian_id FK
    enum consent_status "ConsentStatus"
    enum detail_level "DetailLevel"
    timestamp consented_at
    timestamp revoked_at
  }
  ALLOWANCE_PERIOD {
    uuid id PK
    uuid student_id FK
    decimal starting_amount "financial"
    date start_date
    date next_padala_date
    enum status "AllowancePeriodStatus"
  }
  EXPENSE {
    uuid id PK
    uuid allowance_period_id FK
    decimal amount "financial"
    enum category "ExpenseCategory"
    boolean is_unexpected
    varchar note "PII free text"
    timestamp spent_at
  }
  NUDGE_SETTING {
    uuid id PK
    uuid student_id FK, UK
    int threshold_days
    boolean sms_enabled
    boolean weekend_reminder_enabled
  }
  WEEKLY_SUMMARY {
    uuid id PK
    uuid parent_link_id FK
    uuid allowance_period_id FK
    date week_start
    decimal total_spent "financial"
    decimal unexpected_total "financial"
    enum delivery_status "DeliveryStatus"
    timestamp sent_at
  }
```

**Primary audience:** Database developers and anyone handling personal data

**Risk it reduces:** Wrong keys or cardinalities, and unmarked personal data (PII) being exposed.

**Owner / reviewer (proposed):** Reese Goyal / Clarence B. Ginete
