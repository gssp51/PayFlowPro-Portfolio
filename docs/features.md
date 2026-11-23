# Features

Portfolio demo with fictional data. Not payroll compliance.

## Office web app
- Role-based sign-in and region selector
- Dashboard: active employees, pending and approved timesheets, clocked-in count
- Employees, job sites, shifts and assignments
- Time entries with clock-in/out detail and corrections (reason required, audited)
- Weekly timesheets and approvals (supervisor and HR Admin)
- Payroll processing and payroll summaries (demo overtime rule: hours above 40 at 1.5×)
- Reports: hours and attendance
- Office employees can clock in and out from the web (region clock, no GPS, no job site)

## iPad kiosk (React Native)
- Supervisor-enrolled device; job site and shift setup, then lock
- Employee ID keypad; clears a half-typed ID after inactivity
- Clock in with confirmation; the clock-in window opens shortly before the shift
- Clock out with injury and meal-break questions and an on-screen signature
- Backend-enforced shift selection and automatic lunch rules

## Timesheet workflow
Draft, then approve and submit (locked), then payroll processed. Reopening is HR Admin only, needs a reason, and is audited. A week with open meal-break exceptions cannot be approved.

## Roles
Employee, Supervisor, Superintendent, Admin (HR Admin), VP (read-only payroll in granted regions).

## Not included
Taxes, deductions, payslips, payroll adjustments after processing, production deployment.
