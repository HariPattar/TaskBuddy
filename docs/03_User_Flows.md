# TaskBuddy - User Flows

# 1. Company Onboarding Flow

## Goal

Allow a new company to onboard onto TaskBuddy.

## Flow

Super Admin
↓
Create Company
↓
Assign Subscription Plan
↓
Company Admin Account Created
↓
Company Admin Receives Login Credentials
↓
Company Admin Login
↓
Configure Company Settings
↓
Configure Leave Policies
↓
Configure Holidays
↓
Add Managers
↓
Add Employees
↓
System Ready

---

# 2. Employee Login Flow

Employee
↓
Enter Email
↓
Enter Password
↓
Authentication
↓
Employee Dashboard

If Invalid Credentials

Show Error Message
↓
Retry Login

---

# 3. Employee Attendance Flow

Employee Dashboard
↓
Check In Button
↓
System Records Date & Time
↓
Attendance Status = Present
↓
Employee Works
↓
Check Out Button
↓
System Records Check Out Time
↓
Attendance Record Completed

Rules

* Only one Check In per day
* Check Out requires Check In
* Attendance date generated automatically

---

# 4. Employee Leave Application Flow

Employee Dashboard
↓
Apply Leave
↓
Select Leave Type
↓
Select Start Date
↓
Select End Date
↓
Enter Reason
↓
Submit Request

System Validation

* Check Leave Balance
* Check Policy Rules

If Valid

Leave Status = Pending
↓
Notify Manager

If Invalid

Show Validation Error

---

# 5. Manager Leave Approval Flow

Manager Dashboard
↓
View Pending Requests
↓
Open Request
↓
Review Leave Details

Decision

Approve
OR
Reject

If Approved

Leave Status = Approved
↓
Employee Notified
↓
Leave Balance Updated

If Rejected

Leave Status = Rejected
↓
Employee Notified
↓
Rejection Reason Stored

---

# 6. Manager Attendance Monitoring Flow

Manager Dashboard
↓
View Team Attendance

Display

* Present Employees
* Absent Employees
* Employees On Leave

Manager Can

* Search Employee
* Filter Date Range
* View Team Summary

---

# 7. Company Admin Employee Management Flow

Company Admin Dashboard
↓
Add Employee
↓
Enter Employee Details

Fields

* Name
* Email
* Department
* Designation
* Manager
* Joining Date

Save
↓
Employee Account Created

---

# 8. Company Admin Leave Policy Flow

Company Admin Dashboard
↓
Leave Settings
↓
Create Leave Type

Examples

* Casual Leave
* Sick Leave
* Privilege Leave
* LOP

Configure

* Annual Allocation
* Monthly Limit
* Carry Forward Rule

Save Policy

---

# 9. Company Admin Holiday Management Flow

Company Admin Dashboard
↓
Holiday Management
↓
Add Holiday

Fields

* Holiday Name
* Date
* Description

Save Holiday
↓
Visible To All Employees

---

# 10. Attendance Reporting Flow

Company Admin Dashboard
↓
Reports
↓
Attendance Report

Filters

* Employee
* Department
* Date Range

Generate Report
↓
View Report
↓
Export CSV / Excel

---

# 11. Leave Reporting Flow

Company Admin Dashboard
↓
Reports
↓
Leave Report

Filters

* Employee
* Leave Type
* Department
* Date Range

Generate Report
↓
View Report
↓
Export CSV / Excel

---

# 12. Employee AI Assistant Flow

Employee
↓
Open AI Assistant

Examples

* How many leaves do I have left?
* Show my attendance this month.
* How many sick leaves do I get?

AI
↓
Fetch Relevant Data
↓
Generate Response
↓
Display Answer

---

# 13. Manager AI Assistant Flow

Manager
↓
Open AI Assistant

Examples

* Give me today's team summary.
* Who is on leave next week?
* Show attendance trend.

AI
↓
Fetch Team Data
↓
Generate Summary
↓
Display Answer

---

# 14. Notification Flow

Employee Applies Leave
↓
Manager Notification Sent

Manager Approves Leave
↓
Employee Notification Sent

Manager Rejects Leave
↓
Employee Notification Sent

Employee Misses Attendance
↓
Attendance Reminder Sent
