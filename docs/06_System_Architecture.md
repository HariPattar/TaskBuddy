# TaskBuddy - System Architecture

# Architecture Goals

* Low Cost
* Scalable
* Secure
* Multi-Tenant
* AI Ready
* Easy Maintenance
* SaaS First

---

# High Level Architecture

User
↓
Frontend (Web App)
↓
Backend API
↓
PostgreSQL Database

Additional Services

* AI Service
* Email Service
* WhatsApp Service

---

# Architecture Diagram

Browser
↓
Frontend
↓
FastAPI Backend
↓
PostgreSQL

Optional Services

AI Assistant
↓
OpenAI API

Notifications
↓
Email Provider
↓
WhatsApp Provider

---

# Frontend Architecture

Technology

* Next.js
* TypeScript
* Tailwind CSS
* Shadcn UI

Purpose

* Modern UI
* Mobile Responsive
* Fast Performance
* Component Reusability

Responsibilities

* Authentication Screens
* Dashboard Screens
* Attendance Screens
* Leave Screens
* Reports
* AI Assistant Chat

---

# Backend Architecture

Technology

* Python
* FastAPI

Purpose

* Business Logic
* Authentication
* Authorization
* Leave Calculations
* Attendance Processing
* Reporting APIs
* AI Integration

Responsibilities

* CRUD Operations
* Role Validation
* Multi-Tenant Security
* API Layer

---

# Database Architecture

Technology

* PostgreSQL

Purpose

Store all business data.

Core Tables

* Companies
* Departments
* Users
* Attendance
* Leave Requests
* Leave Balances
* Leave Types
* Holidays
* Notifications
* Audit Logs
* AI Chat History

---

# Multi-Tenant Design

Important Rule

Every business table contains:

company_id

Example

Company A

Employee Data
Attendance Data
Leave Data

Company B

Employee Data
Attendance Data
Leave Data

Data is isolated.

No company can access another company's information.

---

# Authentication Architecture

Technology

* JWT Authentication

Flow

User Login
↓
Email + Password
↓
Backend Validation
↓
JWT Token Generated
↓
Access Granted

Token Used For

* Authentication
* Role Access
* Company Access

---

# Authorization Architecture

Roles

* SUPER_ADMIN
* COMPANY_ADMIN
* MANAGER
* EMPLOYEE

Example

Employee

Can View:

Own Attendance
Own Leaves

Cannot View:

Other Employees

Manager

Can View:

Team Data

Cannot View:

Company Settings

---

# Attendance Architecture

Employee
↓
Check In
↓
Attendance Record Created

Employee
↓
Check Out
↓
Attendance Record Updated

Rules

One Attendance Record Per Day

---

# Leave Management Architecture

Employee
↓
Apply Leave
↓
Validation Layer

Check

* Leave Balance
* Leave Policy

If Valid

Status = Pending

Manager
↓
Approve / Reject

System
↓
Update Leave Balance

---

# Notification Architecture

Events

* Leave Applied
* Leave Approved
* Leave Rejected
* Attendance Missed

Notification Service
↓
Email

Future

↓
WhatsApp

---

# AI Assistant Architecture

Employee Question
↓
Backend
↓
Database Query
↓
AI Service
↓
Response

Examples

How many leaves do I have left?

Show my attendance this month.

How many sick leaves do I get?

Manager Example

Give me today's team summary.

---

# Reporting Architecture

Frontend Filters
↓
Backend API
↓
PostgreSQL Query
↓
Report Generated

Export Formats

* CSV
* Excel

---

# Security Architecture

Password Storage

* BCrypt Hashing

HTTPS

* Enabled

Role-Based Access Control

* Mandatory

Audit Logging

* Mandatory

---

# Hosting Architecture (MVP)

Frontend
↓
Vercel

Backend
↓
Single VPS

Database
↓
PostgreSQL

Benefits

* Low Cost
* Easy Deployment
* Easy Maintenance

---

# Estimated MVP Cost

Domain

₹1000 / Year

Backend VPS

₹500–1000 / Month

Database

Included In VPS

Email

Free Tier Initially

Total

Approx ₹1000–2000 / Month

---

# Scalability Plan

Phase 1

0–50 Companies

Single VPS

---

Phase 2

50–250 Companies

Dedicated Database Server

---

Phase 3

250+ Companies

Separate Services

* API
* Database
* AI Service

---

# Future Architecture

Mobile Application

Android
iOS

Biometric Integration

GPS Attendance

Payroll Module

WhatsApp Bot

Advanced AI Workforce Insights
