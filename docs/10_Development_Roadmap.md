
---

## Phase 0: Planning & Setup

*   **Goal:** Establish your local development environments, initialize version control, and set up project boilerplate structures.
*   **Deliverables:**
    1.  Local PostgreSQL database running in Docker (or installed locally on your system).
    2.  FastAPI backend initialized with Alembic migrations and database models compiled from the ERD design.
    3.  Next.js frontend template initialized with Tailwind CSS and Shadcn UI components.
    4.  Git repository linking both projects.
*   **Dependencies:** None.
*   **Estimated Complexity:** Low (focused on environment setup and configuration).
*   **Estimated Time:** 3 Days.
*   **AI Agent Guidance:** Ask Antigravity to: *"Generate a basic Docker Compose file for local PostgreSQL, initialize a FastAPI project structure with SQLAlchemy, and run the first Alembic migration to create empty tables."*

---

## Phase 1: Authentication

*   **Goal:** Implement secure user registration, login, session validation, and tenant workspace scoping.
*   **Deliverables:**
    1.  Database models and tables for `Companies` and `Users`.
    2.  JSON Web Token (JWT) generator and verification functions in FastAPI.
    3.  Dependency injection logic in backend that pulls the tenant `company_id` directly from the validated JWT token session (Context Resolution Pattern).
    4.  Frontend signup page, login form, and routing middleware that checks user roles and blocks unauthenticated users.
*   **Dependencies:** Phase 0.
*   **Estimated Complexity:** Medium-High (critical for security; token handling must be precise).
*   **Estimated Time:** 7 Days.
*   **AI Agent Guidance:** Ask Antigravity to: *"Build JWT authentication helpers in FastAPI with token verification middleware. Ensure the current tenant company ID is extracted automatically from the token and made available to endpoint functions."*

---

## Phase 2: Company & Employee Management

*   **Goal:** Allow Company Admins to create departments, assign system timezones, add employee profile cards, and link managers.
*   **Deliverables:**
    1.  CRUD endpoints and database tables for `Departments` and `Users` (Lifecycle states: `ACTIVE`, `NOTICE_PERIOD`, etc.).
    2.  Admin profile view to configure company details (timezone setting, phone, email).
    3.  Admin page with an interactive Employee Table directory (displaying designations, departments, managers).
    4.  Admin pop-up forms to add a new employee or deactivate profiles.
*   **Dependencies:** Phase 1.
*   **Estimated Complexity:** Medium (CRUD database operations similar to SQL INSERT/UPDATE steps).
*   **Estimated Time:** 7 Days.
*   **AI Agent Guidance:** Ask Antigravity to: *"Write API endpoints to Create, Read, Update, and Deactivate user profiles. Make sure only users with role COMPANY_ADMIN can call these endpoints."*

---

## Phase 3: Attendance Module

*   **Goal:** Enable check-in/out logging, prevent duplicate entries, and build the supervisor correction request cycle.
*   **Deliverables:**
    1.  `Attendance` table with unique constraint on `(company_id, user_id, attendance_date)`.
    2.  Check-in and Check-out API endpoints that record physical times and determine status (`PRESENT`, `ABSENT`, `HALF_DAY`).
    3.  Attendance Dashboard widget showing today's Check-In/Out timestamps and a monthly calendar grid.
    4.  Correction Request workflow: employee submits adjusted timestamps; manager reviews and approves corrections to update records.
*   **Dependencies:** Phase 2.
*   **Estimated Complexity:** Medium-High (requires careful timestamp and timezone offset conversions).
*   **Estimated Time:** 10 Days.
*   **AI Agent Guidance:** Ask Antigravity to: *"Write check-in and check-out endpoints that calculate if the employee is late, half-day, or present. Store all database entries in UTC, but write a utility function that formats them to the user's localized timezone before returning JSON."*

---

## Phase 4: Leave Management

*   **Goal:** Enable annual leave limits, check remaining balances before applying, and process approvals.
*   **Deliverables:**
    1.  Tables for `LeaveTypes`, `LeaveBalances`, and `LeaveRequests`.
    2.  Leave application form checking current balance pools.
    3.  Manager Approval center listing pending leave requests. Approvals subtract `total_days` from the employee's `LeaveBalances(remaining_days)`.
    4.  Leave request cancellation rules for employees before the start date.
*   **Dependencies:** Phase 2.
*   **Estimated Complexity:** High (requires transactions to avoid double-spend of leave balances).
*   **Estimated Time:** 12 Days.
*   **AI Agent Guidance:** Ask Antigravity to: *"Create a leave application system where the employee checks their remaining balance before submission. Use a database transaction to update the balance table during manager approval to avoid race conditions."*

---

## Phase 5: Reporting

*   **Goal:** Deliver analytics summaries for admins/managers and enable data exports for external spreadsheets.
*   **Deliverables:**
    1.  FastAPI aggregator queries summarizing monthly attendance averages, absent ratios, and total leave allocations.
    2.  Export route in backend that queries attendance history, converts it to CSV using Pandas, and streams the download file.
    3.  Admin KPI Dashboard displaying count cards (Present Today, On Leave, Total Staff) and trend graphs.
*   **Dependencies:** Phase 3, Phase 4.
*   **Estimated Complexity:** Low-Medium (highly familiar for a Data Analyst: SQL aggregation and Pandas export).
*   **Estimated Time:** 5 Days.
*   **AI Agent Guidance:** Ask Antigravity to: *"Write a FastAPI route that aggregates monthly attendance metrics per department. Stream this aggregated output directly as a CSV download link for the front-end dashboard."*

---

## Phase 6: AI Assistant

*   **Goal:** Deploy a natural language interface so employees can ask about leaves, policies, and team summaries.
*   **Deliverables:**
    1.  `AIChatHistory` table tracking chat queries and token metrics.
    2.  AI client integration calling OpenAI's Chat Completion API with strict context routing.
    3.  Context compiler: code extracts user balance data or company calendar and passes it as system prompt context to answer questions.
    4.  UI chat interface showing conversational thread histories.
*   **Dependencies:** Phase 3, Phase 4, Phase 5.
*   **Estimated Complexity:** Medium (relies on clear prompt layouts; security scope checks are critical).
*   **Estimated Time:** 7 Days.
*   **AI Agent Guidance:** Ask Antigravity to: *"Create a secure RAG prompt template. Before calling OpenAI, fetch the caller's specific leave balance details and upcoming holidays, and prepend it to the context so the model answers questions accurately without seeing other users' data."*

---

## Phase 7: Deployment

*   **Goal:** Take TaskBuddy live on production cloud servers under your custom domain.
*   **Deliverables:**
    1.  Frontend statically generated and deployed to Vercel (free hosting tier).
    2.  Backend FastAPI containerized via Docker and deployed to a single VPS (e.g. DigitalOcean or Hetzner - $5-$10/month).
    3.  Nginx configured as reverse proxy with SSL certificate setup (Let's Encrypt / Certbot).
    4.  Automated cron script running nightly database backups to secure object storage.
*   **Dependencies:** All previous phases.
*   **Estimated Complexity:** Medium (standard production configuration steps).
*   **Estimated Time:** 5 Days.
*   **AI Agent Guidance:** Ask Antigravity to: *"Provide a clean dockerfile for the FastAPI backend and guide me step-by-step to set up Nginx reverse proxy with HTTPS on a clean Ubuntu VPS."*
