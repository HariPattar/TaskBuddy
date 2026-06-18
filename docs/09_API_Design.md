
# TaskBuddy - REST API Design Contract

This document specifies the complete REST API contract for the **TaskBuddy** MVP. The API is designed to run over HTTPS, return JSON responses, and utilize stateless JWTs. 

Tenant isolation is handled implicitly by the backend via the authenticated user's session token; the client **never** specifies `company_id` directly in request payloads to prevent cross-tenant parameter injection.

---

## 1. Authentication Module

### Login
*   **Method:** `POST`
*   **Endpoint:** `/api/v1/auth/login`
*   **Purpose:** Authenticate user and issue access token in response (and/or secure HTTP-Only cookie).
*   **Request Body:**
    ```json
    {
      "email": "employee@company.com",
      "password": "SecurePassword123"
    }
    ```
*   **Response Body (200 OK):**
    ```json
    {
      "access_token": "eyJhbGciOi...",
      "token_type": "bearer",
      "expires_in": 3600,
      "user": {
        "user_id": "8f5a2e1d-4b9c-4d3e-8f2a-7b6c5d4e3f2a",
        "first_name": "Jane",
        "last_name": "Doe",
        "email": "employee@company.com",
        "role": "EMPLOYEE"
      }
    }
    ```
*   **Required Role:** `ANONYMOUS`

### Logout
*   **Method:** `POST`
*   **Endpoint:** `/api/v1/auth/logout`
*   **Purpose:** Invalidate the current session token (adds current token to a blacklist).
*   **Request Body:** None
*   **Response Body (200 OK):**
    ```json
    {
      "message": "Logged out successfully"
    }
    ```
*   **Required Role:** `ANY_AUTHENTICATED`

### Password Reset Request
*   **Method:** `POST`
*   **Endpoint:** `/api/v1/auth/forgot-password`
*   **Purpose:** Initiate email password reset.
*   **Request Body:**
    ```json
    {
      "email": "employee@company.com"
    }
    ```
*   **Response Body (202 Accepted):**
    ```json
    {
      "message": "If the email exists, a password reset link has been sent."
    }
    ```
*   **Required Role:** `ANONYMOUS`

### Password Reset Confirm
*   **Method:** `POST`
*   **Endpoint:** `/api/v1/auth/reset-password`
*   **Purpose:** Confirm password reset using verification token.
*   **Request Body:**
    ```json
    {
      "token": "reset_token_from_email",
      "new_password": "NewSecurePassword123"
    }
    ```
*   **Response Body (200 OK):**
    ```json
    {
      "message": "Password updated successfully"
    }
    ```
*   **Required Role:** `ANONYMOUS`

---

## 2. Company Management Module

### Get Company Configuration
*   **Method:** `GET`
*   **Endpoint:** `/api/v1/company`
*   **Purpose:** Retrieve configurations and settings for the authenticated tenant.
*   **Request Body:** None
*   **Response Body (200 OK):**
    ```json
    {
      "company_id": "7b6c5d4e-8f2a-4b9c-4d3e-8f5a2e1d4b9c",
      "company_name": "ACME Corp",
      "company_email": "admin@acme.com",
      "company_phone": "+1234567890",
      "timezone": "Asia/Kolkata",
      "subscription_plan": "FREE",
      "subscription_status": "ACTIVE"
    }
    ```
*   **Required Role:** `COMPANY_ADMIN`

### Update Company Settings
*   **Method:** `PUT`
*   **Endpoint:** `/api/v1/company`
*   **Purpose:** Update settings (e.g. business timezone).
*   **Request Body:**
    ```json
    {
      "company_name": "ACME Corporation",
      "company_phone": "+1987654321",
      "timezone": "America/New_York"
    }
    ```
*   **Response Body (200 OK):**
    ```json
    {
      "message": "Company updated successfully",
      "company": {
        "company_id": "7b6c5d4e-8f2a-4b9c-4d3e-8f5a2e1d4b9c",
        "company_name": "ACME Corporation",
        "timezone": "America/New_York"
      }
    }
    ```
*   **Required Role:** `COMPANY_ADMIN`

---

## 3. Department Management Module

### List Departments
*   **Method:** `GET`
*   **Endpoint:** `/api/v1/departments`
*   **Purpose:** List all departments under this company.
*   **Request Body:** None
*   **Response Body (200 OK):**
    ```json
    [
      {
        "department_id": "9d8c7b6a-5e4d-3c2b-1a0f-9e8d7c6b5a4f",
        "department_name": "Engineering",
        "created_at": "2026-06-01T00:00:00Z"
      },
      {
        "department_id": "1a2b3c4d-5e6f-7a8b-9c0d-1e2f3a4b5c6d",
        "department_name": "Sales",
        "created_at": "2026-06-01T00:00:00Z"
      }
    ]
    ```
*   **Required Role:** `ANY_AUTHENTICATED`

### Create Department
*   **Method:** `POST`
*   **Endpoint:** `/api/v1/departments`
*   **Purpose:** Create a new department.
*   **Request Body:**
    ```json
    {
      "department_name": "Human Resources"
    }
    ```
*   **Response Body (201 Created):**
    ```json
    {
      "department_id": "a1b2c3d4-e5f6-7a8b-9c0d-1e2f3a4b5c6d",
      "department_name": "Human Resources"
    }
    ```
*   **Required Role:** `COMPANY_ADMIN`

### Delete Department
*   **Method:** `DELETE`
*   **Endpoint:** `/api/v1/departments/{department_id}`
*   **Purpose:** Delete a department (Users attached will have department reference set to null).
*   **Request Body:** None
*   **Response Body (200 OK):**
    ```json
    {
      "message": "Department deleted successfully"
    }
    ```
*   **Required Role:** `COMPANY_ADMIN`

---

## 4. Employee Management Module

### List Employees
*   **Method:** `GET`
*   **Endpoint:** `/api/v1/employees`
*   **Purpose:** Paginated retrieval of company staff. Filters by department or status.
*   **Request Body:** None
*   **Query Parameters:** `page=1`, `limit=50`, `department_id=optional_uuid`, `status=ACTIVE`
*   **Response Body (200 OK):**
    ```json
    {
      "data": [
        {
          "user_id": "8f5a2e1d-4b9c-4d3e-8f2a-7b6c5d4e3f2a",
          "first_name": "Jane",
          "last_name": "Doe",
          "email": "jane@acme.com",
          "role": "EMPLOYEE",
          "designation": "Backend Engineer",
          "department": {
            "department_id": "9d8c7b6a-5e4d-3c2b-1a0f-9e8d7c6b5a4f",
            "department_name": "Engineering"
          },
          "manager_id": "5a4b3c2d-1e0f-9a8b-7c6d-5e4f3a2b1c0d",
          "employee_status": "ACTIVE",
          "joining_date": "2025-01-15"
        }
      ],
      "total_count": 12,
      "page": 1,
      "pages": 1
    }
    ```
*   **Required Role:** `MANAGER` or `COMPANY_ADMIN`

### Create Employee
*   **Method:** `POST`
*   **Endpoint:** `/api/v1/employees`
*   **Purpose:** Create a new employee profile.
*   **Request Body:**
    ```json
    {
      "first_name": "John",
      "last_name": "Smith",
      "email": "john.smith@acme.com",
      "password": "TemporaryPassword123",
      "role": "EMPLOYEE",
      "designation": "Accountant",
      "department_id": "1a2b3c4d-5e6f-7a8b-9c0d-1e2f3a4b5c6d",
      "manager_id": "8f5a2e1d-4b9c-4d3e-8f2a-7b6c5d4e3f2a",
      "joining_date": "2026-06-20"
    }
    ```
*   **Response Body (201 Created):**
    ```json
    {
      "user_id": "3c4d5e6f-7a8b-9c0d-1e2f-3a4b5c6d7e8f",
      "email": "john.smith@acme.com",
      "message": "Employee created successfully"
    }
    ```
*   **Required Role:** `COMPANY_ADMIN`

### Update Employee Details
*   **Method:** `PUT`
*   **Endpoint:** `/api/v1/employees/{user_id}`
*   **Purpose:** Edit employee profile (department, designation, manager, or status).
*   **Request Body:**
    ```json
    {
      "designation": "Lead Accountant",
      "manager_id": "7b6c5d4e-8f2a-4b9c-4d3e-8f5a2e1d4b9c",
      "employee_status": "ACTIVE"
    }
    ```
*   **Response Body (200 OK):**
    ```json
    {
      "message": "Employee profile updated successfully"
    }
    ```
*   **Required Role:** `COMPANY_ADMIN`

---

## 5. Attendance Module

### Check In
*   **Method:** `POST`
*   **Endpoint:** `/api/v1/attendance/check-in`
*   **Purpose:** Record check-in timestamp for today.
*   **Request Body:** None
*   **Response Body (200 OK):**
    ```json
    {
      "attendance_id": "fa2b3c4d-5e6f-7a8b-9c0d-1e2f3a4b5c6d",
      "attendance_date": "2026-06-18",
      "check_in_time": "2026-06-18T09:00:15+05:30",
      "check_out_time": null,
      "attendance_status": "PRESENT"
    }
    ```
*   **Required Role:** `ANY_AUTHENTICATED`

### Check Out
*   **Method:** `POST`
*   **Endpoint:** `/api/v1/attendance/check-out`
*   **Purpose:** Record check-out timestamp.
*   **Request Body:** None
*   **Response Body (200 OK):**
    ```json
    {
      "attendance_id": "fa2b3c4d-5e6f-7a8b-9c0d-1e2f3a4b5c6d",
      "check_in_time": "2026-06-18T09:00:15+05:30",
      "check_out_time": "2026-06-18T18:05:42+05:30",
      "attendance_status": "PRESENT"
    }
    ```
*   **Required Role:** `ANY_AUTHENTICATED`

### Get Own Attendance History
*   **Method:** `GET`
*   **Endpoint:** `/api/v1/attendance/my`
*   **Purpose:** List logs for the authenticated user within date range.
*   **Request Body:** None
*   **Query Parameters:** `start_date=2026-06-01`, `end_date=2026-06-30`
*   **Response Body (200 OK):**
    ```json
    [
      {
        "attendance_date": "2026-06-18",
        "check_in_time": "2026-06-18T09:00:15+05:30",
        "check_out_time": "2026-06-18T18:05:42+05:30",
        "attendance_status": "PRESENT"
      }
    ]
    ```
*   **Required Role:** `ANY_AUTHENTICATED`

---

## 6. Attendance Corrections Module

### Submit Correction Request
*   **Method:** `POST`
*   **Endpoint:** `/api/v1/attendance/corrections`
*   **Purpose:** Submit request for missed or incorrect check-in/out.
*   **Request Body:**
    ```json
    {
      "attendance_id": "fa2b3c4d-5e6f-7a8b-9c0d-1e2f3a4b5c6d",
      "requested_check_in": "2026-06-18T08:55:00+05:30",
      "requested_check_out": "2026-06-18T18:00:00+05:30",
      "reason": "Forgot to check in, worked standard shift"
    }
    ```
*   **Response Body (201 Created):**
    ```json
    {
      "correction_id": "c1d2e3f4-5a6b-7c8d-9e0f-1a2b3c4d5e6f",
      "status": "PENDING"
    }
    ```
*   **Required Role:** `ANY_AUTHENTICATED`

### List Pending Corrections
*   **Method:** `GET`
*   **Endpoint:** `/api/v1/attendance/corrections/pending`
*   **Purpose:** View pending correction requests submitted by direct reports.
*   **Request Body:** None
*   **Response Body (200 OK):**
    ```json
    [
      {
        "correction_id": "c1d2e3f4-5a6b-7c8d-9e0f-1a2b3c4d5e6f",
        "user_id": "3c4d5e6f-7a8b-9c0d-1e2f-3a4b5c6d7e8f",
        "employee_name": "John Smith",
        "attendance_date": "2026-06-18",
        "requested_check_in": "2026-06-18T08:55:00+05:30",
        "requested_check_out": "2026-06-18T18:00:00+05:30",
        "reason": "Forgot to check in, worked standard shift",
        "created_at": "2026-06-18T19:00:00Z"
      }
    ]
    ```
*   **Required Role:** `MANAGER` or `COMPANY_ADMIN`

### Resolve Correction Request
*   **Method:** `POST`
*   **Endpoint:** `/api/v1/attendance/corrections/{correction_id}/resolve`
*   **Purpose:** Approve or reject a correction request (approvals automatically correct target Attendance record).
*   **Request Body:**
    ```json
    {
      "status": "APPROVED",
      "rejection_reason": null
    }
    ```
*   **Response Body (200 OK):**
    ```json
    {
      "message": "Correction request has been APPROVED"
    }
    ```
*   **Required Role:** `MANAGER` or `COMPANY_ADMIN`

---

## 7. Leave Management Module

### List Leave Types
*   **Method:** `GET`
*   **Endpoint:** `/api/v1/leaves/types`
*   **Purpose:** Retrieve list of active leave policies.
*   **Request Body:** None
*   **Response Body (200 OK):**
    ```json
    [
      {
        "leave_type_id": "c7b6a5e4-3d2c-1b0a-9f8e-7d6c5b4a3f2e",
        "leave_name": "Casual Leave",
        "annual_limit": 12,
        "monthly_limit": 2,
        "carry_forward_allowed": false,
        "half_day_allowed": true,
        "negative_balance_allowed": false
      }
    ]
    ```
*   **Required Role:** `ANY_AUTHENTICATED`

### Create/Update Leave Type
*   **Method:** `POST`
*   **Endpoint:** `/api/v1/leaves/types`
*   **Purpose:** Configure a new leave type. (Use `PUT` on `/api/v1/leaves/types/{id}` to modify).
*   **Request Body:**
    ```json
    {
      "leave_name": "Sick Leave",
      "annual_limit": 10,
      "monthly_limit": null,
      "carry_forward_allowed": true,
      "half_day_allowed": true,
      "negative_balance_allowed": true
    }
    ```
*   **Response Body (201 Created):**
    ```json
    {
      "leave_type_id": "d8e9f0a1-b2c3-4d5e-6f7a-8b9c0d1e2f3a",
      "leave_name": "Sick Leave"
    }
    ```
*   **Required Role:** `COMPANY_ADMIN`

### Get Leave Balances
*   **Method:** `GET`
*   **Endpoint:** `/api/v1/leaves/balances`
*   **Purpose:** Retrieve current leave balances for the caller.
*   **Request Body:** None
*   **Query Parameters:** `year=2026`
*   **Response Body (200 OK):**
    ```json
    [
      {
        "leave_type_id": "c7b6a5e4-3d2c-1b0a-9f8e-7d6c5b4a3f2e",
        "leave_name": "Casual Leave",
        "allocated_days": 12.0,
        "used_days": 2.5,
        "remaining_days": 9.5
      }
    ]
    ```
*   **Required Role:** `ANY_AUTHENTICATED` (Managers can pass `user_id` query parameter to check team balances).

### Submit Leave Request
*   **Method:** `POST`
*   **Endpoint:** `/api/v1/leaves/requests`
*   **Purpose:** Apply for leave. Validates balance before saving status as PENDING.
*   **Request Body:**
    ```json
    {
      "leave_type_id": "c7b6a5e4-3d2c-1b0a-9f8e-7d6c5b4a3f2e",
      "start_date": "2026-07-10",
      "end_date": "2026-07-12",
      "total_days": 3.0,
      "reason": "Family trip"
    }
    ```
*   **Response Body (201 Created):**
    ```json
    {
      "leave_request_id": "e9f0a1b2-c3d4-4d5e-6f7a-8b9c0d1e2f3a",
      "status": "PENDING"
    }
    ```
*   **Required Role:** `ANY_AUTHENTICATED`

### Resolve Leave Request
*   **Method:** `POST`
*   **Endpoint:** `/api/v1/leaves/requests/{leave_request_id}/resolve`
*   **Purpose:** Approve or reject leave request.
*   **Request Body:**
    ```json
    {
      "status": "APPROVED",
      "rejection_reason": null
    }
    ```
*   **Response Body (200 OK):**
    ```json
    {
      "message": "Leave request status updated to APPROVED"
    }
    ```
*   **Required Role:** `MANAGER` or `COMPANY_ADMIN`

---

## 8. Holidays Module

### List Holidays
*   **Method:** `GET`
*   **Endpoint:** `/api/v1/holidays`
*   **Purpose:** Get calendar of official holidays.
*   **Request Body:** None
*   **Response Body (200 OK):**
    ```json
    [
      {
        "holiday_id": "h1i2j3k4-5l6m-7n8o-9p0q-1r2s3t4u5v6w",
        "holiday_name": "Christmas Day",
        "holiday_date": "2026-12-25",
        "description": "National Holiday"
      }
    ]
    ```
*   **Required Role:** `ANY_AUTHENTICATED`

### Create/Update Holiday
*   **Method:** `POST`
*   **Endpoint:** `/api/v1/holidays`
*   **Purpose:** Define a holiday. (Use `PUT` on `/api/v1/holidays/{id}` to modify, `DELETE` to remove).
*   **Request Body:**
    ```json
    {
      "holiday_name": "Independence Day",
      "holiday_date": "2026-08-15",
      "description": "Public Holiday"
    }
    ```
*   **Response Body (201 Created):**
    ```json
    {
      "holiday_id": "a1b2c3d4-e5f6-7a8b-9c0d-1e2f3a4b5c6d",
      "holiday_name": "Independence Day"
    }
    ```
*   **Required Role:** `COMPANY_ADMIN`

---

## 9. Reports Module

### Get Company Dashboard KPIs
*   **Method:** `GET`
*   **Endpoint:** `/api/v1/reports/dashboard-kpis`
*   **Purpose:** Fetch contextual dashboard metrics.
*   **Request Body:** None
*   **Response Body (200 OK - Admin View):**
    ```json
    {
      "total_employees": 100,
      "present_today": 82,
      "on_leave_today": 5,
      "absent_today": 13,
      "pending_leave_requests": 3
    }
    ```
*   **Required Role:** `ANY_AUTHENTICATED` (KPI payload keys change depending on User Role).

### Export Attendance Report
*   **Method:** `GET`
*   **Endpoint:** `/api/v1/reports/attendance/export`
*   **Purpose:** Stream generated CSV/Excel file of attendance records.
*   **Request Body:** None
*   **Query Parameters:** `start_date=2026-06-01`, `end_date=2026-06-30`, `format=csv`
*   **Response Headers:** `Content-Type: text/csv`, `Content-Disposition: attachment; filename=attendance_report.csv`
*   **Response Body:** Binary CSV data stream.
*   **Required Role:** `MANAGER` or `COMPANY_ADMIN`

---

## 10. Notifications Module

### List Notifications
*   **Method:** `GET`
*   **Endpoint:** `/api/v1/notifications`
*   **Purpose:** Retrieve list of user notifications.
*   **Request Body:** None
*   **Response Body (200 OK):**
    ```json
    [
      {
        "notification_id": "n1o2p3q4-5r6s-7t8u-9v0w-1x2y3z4a5b6c",
        "notification_type": "LEAVE_APPROVED",
        "message": "Your leave request for 2026-07-10 has been approved.",
        "is_read": false,
        "created_at": "2026-06-18T12:00:00Z"
      }
    ]
    ```
*   **Required Role:** `ANY_AUTHENTICATED`

### Mark Read
*   **Method:** `POST`
*   **Endpoint:** `/api/v1/notifications/{notification_id}/read`
*   **Purpose:** Mark notification as read.
*   **Request Body:** None
*   **Response Body (200 OK):**
    ```json
    {
      "message": "Notification marked as read"
    }
    ```
*   **Required Role:** `ANY_AUTHENTICATED`

---

## 11. AI Assistant Module

### Query AI Assistant
*   **Method:** `POST`
*   **Endpoint:** `/api/v1/ai/chat`
*   **Purpose:** Submit a natural language question. Handled as a Server-Sent Event stream.
*   **Request Body:**
    ```json
    {
      "question": "How many leaves do I have left?"
    }
    ```
*   **Response Headers:** `Content-Type: text/event-stream`, `Cache-Control: no-cache`
*   **Response Body:**
    ```
    data: {"chunk": "You "}
    data: {"chunk": "have "}
    data: {"chunk": "9.5 "}
    data: {"chunk": "days "}
    data: {"chunk": "of "}
    data: {"chunk": "Casual "}
    data: {"chunk": "Leave "}
    data: {"chunk": "remaining."}
    event: end
    data: [DONE]
    ```
*   **Required Role:** `ANY_AUTHENTICATED`
