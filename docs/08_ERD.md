# TaskBuddy - Entity-Relationship Diagram (ERD)

This document outlines the production-ready relational database design for the **TaskBuddy** MVP. The schema is designed for PostgreSQL as a multi-tenant SaaS application using logical separation via a `company_id` column.

```mermaid
erDiagram
    COMPANIES {
        uuid company_id PK
        varchar company_name
        varchar company_email
        varchar company_phone
        varchar subscription_plan
        varchar subscription_status
        varchar timezone
        timestamp created_at
        timestamp updated_at
    }

    DEPARTMENTS {
        uuid department_id PK
        uuid company_id FK
        varchar department_name
        timestamp created_at
    }

    USERS {
        uuid user_id PK
        uuid company_id FK
        uuid department_id FK
        uuid manager_id FK
        varchar first_name
        varchar last_name
        varchar email
        varchar password_hash
        varchar role
        varchar designation
        varchar employee_status
        date joining_date
        timestamp created_at
        timestamp updated_at
    }

    ATTENDANCE {
        uuid attendance_id PK
        uuid company_id FK
        uuid user_id FK
        date attendance_date
        timestamptz check_in_time
        timestamptz check_out_time
        varchar attendance_status
        timestamp created_at
    }

    ATTENDANCE_CORRECTIONS {
        uuid correction_id PK
        uuid company_id FK
        uuid attendance_id FK
        uuid user_id FK
        timestamptz requested_check_in
        timestamptz requested_check_out
        varchar reason
        varchar status
        uuid reviewed_by FK
        timestamp reviewed_at
        varchar rejection_reason
        timestamp created_at
    }

    LEAVE_TYPES {
        uuid leave_type_id PK
        uuid company_id FK
        varchar leave_name
        integer annual_limit
        integer monthly_limit
        boolean carry_forward_allowed
        boolean half_day_allowed
        boolean negative_balance_allowed
        timestamp created_at
    }

    LEAVE_BALANCES {
        uuid leave_balance_id PK
        uuid company_id FK
        uuid user_id FK
        uuid leave_type_id FK
        decimal allocated_days
        decimal used_days
        decimal remaining_days
        integer year
        timestamp created_at
    }

    LEAVE_REQUESTS {
        uuid leave_request_id PK
        uuid company_id FK
        uuid user_id FK
        uuid leave_type_id FK
        date start_date
        date end_date
        decimal total_days
        varchar reason
        varchar status
        uuid approved_by FK
        timestamp approved_at
        varchar rejection_reason
        timestamp created_at
    }

    HOLIDAYS {
        uuid holiday_id PK
        uuid company_id FK
        varchar holiday_name
        date holiday_date
        varchar description
        timestamp created_at
    }

    NOTIFICATIONS {
        uuid notification_id PK
        uuid company_id FK
        uuid user_id FK
        varchar notification_type
        text message
        boolean is_read
        timestamp created_at
    }

    AUDIT_LOGS {
        uuid audit_log_id PK
        uuid company_id FK
        uuid user_id FK
        varchar action
        varchar entity_name
        uuid entity_id
        jsonb old_values
        jsonb new_values
        timestamp created_at
    }

    AI_CHAT_HISTORY {
        uuid chat_id PK
        uuid company_id FK
        uuid user_id FK
        text question
        text response
        integer tokens_used
        timestamp created_at
    }

    COMPANIES ||--o{ DEPARTMENTS : has
    COMPANIES ||--o{ USERS : owns
    DEPARTMENTS ||--o{ USERS : contains
    USERS ||--o{ USERS : manages
    COMPANIES ||--o{ ATTENDANCE : has
    USERS ||--o{ ATTENDANCE : performs
    COMPANIES ||--o{ ATTENDANCE_CORRECTIONS : has
    ATTENDANCE ||--o{ ATTENDANCE_CORRECTIONS : corrects
    USERS ||--o{ ATTENDANCE_CORRECTIONS : requests
    USERS ||--o{ ATTENDANCE_CORRECTIONS : reviews
    COMPANIES ||--o{ LEAVE_TYPES : has
    COMPANIES ||--o{ LEAVE_BALANCES : has
    USERS ||--o{ LEAVE_BALANCES : owns
    LEAVE_TYPES ||--o{ LEAVE_BALANCES : tracks
    COMPANIES ||--o{ LEAVE_REQUESTS : has
    USERS ||--o{ LEAVE_REQUESTS : requests
    USERS ||--o{ LEAVE_REQUESTS : approves
    LEAVE_TYPES ||--o{ LEAVE_REQUESTS : type_of
    COMPANIES ||--o{ HOLIDAYS : has
    COMPANIES ||--o{ NOTIFICATIONS : has
    USERS ||--o{ NOTIFICATIONS : receives
    COMPANIES ||--o{ AUDIT_LOGS : has
    USERS ||--o{ AUDIT_LOGS : performs
    COMPANIES ||--o{ AI_CHAT_HISTORY : has
    USERS ||--o{ AI_CHAT_HISTORY : converses
