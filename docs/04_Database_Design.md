# TaskBuddy - Database Design

# Database Strategy

Multi-Tenant SaaS

Every table must contain:

company_id

This ensures data isolation between companies.

---

# 1. Companies

Purpose

Store customer companies.

Fields

* company_id (PK)
* company_name
* company_email
* company_phone
* subscription_plan
* subscription_status
* created_at
* updated_at

Relationships

One Company
→ Many Employees
→ Many Managers
→ Many Leave Policies

---

# 2. Departments

Purpose

Store departments within a company.

Fields

* department_id (PK)
* company_id (FK)
* department_name
* created_at

Examples

* Sales
* Operations
* HR
* Finance

---

# 3. Users

Purpose

Master user table.

Fields

* user_id (PK)

* company_id (FK)

* department_id (FK)

* manager_id (FK - nullable)

* first_name

* last_name

* email

* password_hash

* role

  * COMPANY_ADMIN
  * MANAGER
  * EMPLOYEE

* designation

* employee_status

  * ACTIVE
  * NOTICE_PERIOD
  * RESIGNED
  * TERMINATED
  * INACTIVE

* joining_date

* created_at

* updated_at

---

# 4. Leave Types

Purpose

Store company-specific leave types.

Fields

* leave_type_id (PK)

* company_id (FK)

* leave_name

Examples

* Casual Leave

* Sick Leave

* Privilege Leave

* Loss Of Pay

* annual_limit

* monthly_limit

* carry_forward_allowed

* half_day_allowed

* negative_balance_allowed

* created_at

---

# 5. Leave Balances

Purpose

Track available leaves.

Fields

* leave_balance_id (PK)

* company_id (FK)

* user_id (FK)

* leave_type_id (FK)

* allocated_days

* used_days

* remaining_days

* year

Relationships

One Employee
→ Many Leave Balances

---

# 6. Leave Requests

Purpose

Track leave applications.

Fields

* leave_request_id (PK)

* company_id (FK)

* user_id (FK)

* leave_type_id (FK)

* start_date

* end_date

* total_days

* reason

* status

Values

* PENDING

* APPROVED

* REJECTED

* approved_by

* approved_at

* rejection_reason

* created_at

---

# 7. Attendance

Purpose

Track daily attendance.

Fields

* attendance_id (PK)

* company_id (FK)

* user_id (FK)

* attendance_date

* check_in_time

* check_out_time

* attendance_status

Values

* PRESENT

* ABSENT

* HALF_DAY

* ON_LEAVE

* HOLIDAY

* WEEK_OFF

* created_at

Rules

One attendance record per employee per day.

---

# 8. Holidays

Purpose

Store company holidays.

Fields

* holiday_id (PK)

* company_id (FK)

* holiday_name

* holiday_date

* description

* created_at

---

# 9. Notifications

Purpose

Store notification history.

Fields

* notification_id (PK)

* company_id (FK)

* user_id (FK)

* notification_type

Examples

* LEAVE_APPROVED

* LEAVE_REJECTED

* LEAVE_REQUESTED

* ATTENDANCE_MISSED

* message

* is_read

* created_at

---

# 10. Audit Logs

Purpose

Track system activities.

Fields

* audit_log_id (PK)

* company_id (FK)

* user_id (FK)

* action

Examples

* CREATE_EMPLOYEE

* UPDATE_POLICY

* APPROVE_LEAVE

* REJECT_LEAVE

* entity_name

* entity_id

* created_at

---

# 11. AI Chat History

Purpose

Store AI conversations.

Fields

* chat_id (PK)

* company_id (FK)

* user_id (FK)

* question

* response

* created_at

---

# Relationships Summary

Company
│
├── Departments
├── Users
├── Leave Types
├── Holidays
├── Attendance
├── Leave Requests
├── Leave Balances
├── Notifications
├── Audit Logs
└── AI Chat History

Department
│
└── Users

User
│
├── Attendance
├── Leave Requests
├── Leave Balances
├── Notifications
└── AI Chat History
