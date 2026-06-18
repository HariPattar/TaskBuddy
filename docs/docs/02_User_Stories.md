# TaskBuddy - User Stories

## Employee Module

### Login

As an employee,
I want to login to the system,
So that I can access my attendance and leave information.

### Acceptance Criteria

* Login using email and password
* Redirect to employee dashboard
* Show error for invalid credentials

---

### Check In

As an employee,
I want to check in for work,
So that my attendance is recorded.

### Acceptance Criteria

* Check In button available
* Attendance date captured automatically
* Check In time captured automatically
* Only one Check In allowed per day

---

### Check Out

As an employee,
I want to check out after work,
So that my working day is completed.

### Acceptance Criteria

* Check Out button available
* Check Out time captured automatically
* Cannot Check Out before Check In

---

### Apply Leave

As an employee,
I want to apply for leave,
So that I can request time off.

### Acceptance Criteria

* Select leave type
* Select start date
* Select end date
* Enter reason
* Validate leave balance
* Submit request

---

### View Leave Balance

As an employee,
I want to view my leave balance,
So that I know how many leaves remain.

### Acceptance Criteria

* Show balance by leave type
* Show used leaves
* Show remaining leaves

---

### View Attendance History

As an employee,
I want to see my attendance history,
So that I can track my attendance records.

### Acceptance Criteria

* Filter by date range
* View Check In and Check Out times

---

## Manager Module

### Approve Leave

As a manager,
I want to approve leave requests,
So that employee leave can be processed.

### Acceptance Criteria

* View pending requests
* Approve request
* Notify employee

---

### Reject Leave

As a manager,
I want to reject leave requests,
So that invalid requests can be denied.

### Acceptance Criteria

* Reject request
* Enter rejection reason
* Notify employee

---

### Team Attendance Dashboard

As a manager,
I want to view team attendance,
So that I can monitor workforce availability.

### Acceptance Criteria

* Present employees
* Absent employees
* Employees on leave
* Attendance summary

---

## Company Admin Module

### Add Employee

As a company admin,
I want to add employees,
So that they can access the system.

### Acceptance Criteria

* Create employee profile
* Assign department
* Assign manager

---

### Configure Leave Policies

As a company admin,
I want to configure leave policies,
So that company rules can be managed.

### Acceptance Criteria

* Create leave types
* Define leave limits
* Define leave rules

---

### View Reports

As a company admin,
I want to view attendance and leave reports,
So that I can monitor workforce activity.

### Acceptance Criteria

* Attendance reports
* Leave reports
* Export functionality
