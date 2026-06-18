# TaskBuddy - Product Requirements Document (PRD)

## 1. Product Information

### Product Name
TaskBuddy

### Product Vision
To make workforce management simple, affordable, and intelligent for small businesses by providing a centralized platform for attendance, leave tracking, approvals, reporting, and AI-powered employee insights.

---

## 2. Problem Statement

Small businesses often rely on spreadsheets, WhatsApp messages, and manual processes to manage employee attendance and leave records. These methods are prone to errors, difficult to scale, provide limited visibility into workforce availability, and consume valuable administrative time.

---

## 3. Target Customers

- Startups
- Educational Institutes
- Retail Chains
- Service Businesses
- Agencies
- Any company with less than 100 employees

---

## 4. Employee Size

The first version is designed for companies with up to 100 employees. The architecture should support future expansion to larger organizations without major redesign.

---

## 5. User Roles & Responsibilities

### Super Admin (Platform Owner)

Responsibilities:
- Manage company accounts
- Activate/deactivate subscriptions
- Manage plans and pricing
- View platform-wide analytics
- Configure platform settings
- Manage AI configuration
- Monitor usage and support requests
- Access all audit logs

Permissions:
- Full platform access

### Company Admin

Responsibilities:
- Manage employees and managers
- Configure attendance policies
- Configure leave policies
- Configure leave types and limits
- Configure holidays
- Configure departments
- Configure office timings
- View company-wide reports
- Override leave requests when required
- Manage company settings

Permissions:
- Full access within own company

### Manager

Responsibilities:
- Approve/reject leave requests
- View team attendance
- View team leave calendar
- View employee leave balances
- Generate team reports
- Monitor absenteeism

Permissions:
- Access only to assigned team members

### Employee

Responsibilities:
- Mark attendance
- Apply/cancel leave
- View attendance history
- View leave balance
- View leave history
- View holidays
- Update profile
- Use AI assistant

Permissions:
- Access only to personal information

---

## 6. Company Structure

### Department Management

Company Admin can:

- Create departments
- Edit departments
- Assign employees to departments
- Assign managers to departments

Examples:

- Sales
- Operations
- HR
- Finance
- IT

---

## 7. Employee Lifecycle

Employee Status:

- Active
- On Notice Period
- Resigned
- Terminated
- Inactive

---

## 8. Attendance Requirements

### Attendance Method

- Manual Check-In
- Manual Check-Out

### Attendance Status

- Present
- Absent
- Half Day
- Holiday
- Week Off

### Future Attendance Methods

- GPS Based Attendance
- QR Attendance
- Biometric Integration
- Face Recognition

---

## 9. Leave Management

### Leave Types

- Casual Leave
- Sick Leave
- Privilege Leave
- Loss of Pay (LOP)

### Leave Workflow

Employee → Manager → Pending / Approved / Rejected

Manager Leave:

- Auto Approved

### Leave Policy Configuration

Company Admin can configure:

- Annual Leave Allocation
- Monthly Leave Limit
- Carry Forward Rules
- Half-Day Leave Rules
- Negative Balance Rules
- Encashment Rules

---

## 10. Holiday Management

Company Admin can:

- Create Holidays
- Edit Holidays
- Delete Holidays

Fields:

- Holiday Name
- Date
- Description

---

## 11. Reporting Requirements

### Employee Dashboard

KPIs:

- Total Leaves Available
- Leaves Used
- Remaining Leave Balance
- Upcoming Holidays

Reports:

- Attendance History
- Leave History
- Monthly Attendance Calendar

### Manager Dashboard

KPIs:

- My Leave Balance
- Pending Leave Requests
- Employees On Leave Today
- Team Attendance Today

Reports:

- My Attendance History
- My Leave History
- Team Leave Requests
- Team Leave Calendar

### Company Admin Dashboard

KPIs:

- Total Employees
- Employees Present Today
- Employees On Leave Today
- Pending Leave Requests

Reports:

- Attendance Summary
- Leave Summary
- Employee Attendance Report
- Employee Leave Report
- Upcoming Leave Calendar

Quick Actions:

- Add Employee
- Add Manager
- Configure Leave Types
- Configure Leave Limits
- Configure Holidays
- Export Reports

---

## 12. Notifications

### Channels

- Email
- WhatsApp

### Employee Notifications

- Leave Approved
- Leave Rejected
- Attendance Missed

### Manager Notifications

- New Leave Request Submitted

---

## 13. AI Features

### Employee AI Assistant

- Leave Balance Queries
- Attendance Queries
- Company Policy Queries

### Manager AI Assistant

- Team Summary
- Team Attendance Overview
- Upcoming Leave Planning
- Absenteeism Insights

### Future AI Features

Phase 1:
- Leave Balance
- Attendance Queries
- Policy Questions

Phase 2:
- Team Availability Insights
- Leave Planning Suggestions
- Attendance Trends

Phase 3:
- Workforce Forecasting
- Leave Exhaustion Prediction
- Department Attendance Analysis

---

## 14. Security Requirements

### Authentication

- Email + Password Login
- JWT Authentication
- Password Encryption

Future:

- Google Login

---

## 15. Audit Logs

Track:

- Leave Applied
- Leave Approved
- Leave Rejected
- Employee Added
- Employee Updated
- Policy Changes

Fields:

- User
- Action
- Timestamp
- Entity

---

## 16. SaaS Requirements

### Multi-Tenant Architecture

Supported.

Every company must have isolated data.

### Subscription Buckets

- 0–50 Employees
- 51–100 Employees
- 101–250 Employees

Pricing:
- Direct Sales / Custom Pricing

---

## 17. Success Criteria

- Reduce manual leave tracking by 90%
- Automate attendance reporting
- Onboard a company in under 5 minutes
- Support multiple companies from a single platform
- Deliver useful AI-powered workforce insights

---

## 18. Technical Architecture Principles

- Multi-tenant from Day 1
- Mobile Responsive Web Application
- FastAPI Backend
- PostgreSQL Database
- Scalable SaaS Architecture
- Low Operational Cost
- AI-ready Architecture
