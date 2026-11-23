# Architecture

Portfolio demo with fictional data. Not payroll or wage-and-hour compliance. The source code is private; this page describes the design.

## Solution layout

| Project | Role |
|---|---|
| `PayFlowPro.Api` | ASP.NET Core Web API: controllers, authorization policies, shift and clock-in rules, audit logging |
| `PayFlowPro.Core` | Entities, shift rules, `PayrollCalculator` (no web or database dependencies) |
| `PayFlowPro.Infrastructure` | EF Core `DbContext`, ASP.NET Core Identity, migrations |
| `PayFlowPro.Web` | ASP.NET Core MVC office app; calls the API through a typed HTTP client and holds no business rules |
| `PayFlowPro.Tests` | xUnit tests |
| `PayFlowProMobile` | React Native (JavaScript) shared-iPad kiosk |

## Design rules

1. The web app and the iPad app use the same REST API and the same SQL Server database.
2. Neither front end connects to SQL Server.
3. Payroll and time rules live only in the C# backend, never in JavaScript.
4. Controllers receive services through constructor dependency injection; the API's setup registers controllers, the `DbContext`, the payroll calculator and the authorization policies in one place.

## Data (EF Core)

The `DbContext` covers employees, regions and job sites, shifts, time entries, timesheets, payroll records, device enrollments and audit logs. A payroll record has a one-to-one relationship with its timesheet. Time entries have configured precision and a relationship to their timesheet. Schema changes are EF Core migrations.

## Two workflows

| | Office (web) | Field (iPad kiosk) |
|---|---|---|
| Identity | Own username and password | Employee ID only; the iPad is authorized by a supervisor |
| Clocking | Personal clock, region time zone, no job site | Job site, selected shift, geofence |
| Review | HR Admin approves | Site supervisor approves and submits |
| Payroll | HR Admin | HR Admin (supervisors cannot process payroll) |

## Security

- JWT bearer tokens and ASP.NET Core Identity for users; device token plus supervisor PIN for kiosks (hashed at rest, with lockout counters).
- Role- and region-scoped authorization policies, enforced on the API so the web app, the iPad app and direct API calls behave the same.
- Roles: Employee, Supervisor, Superintendent, Admin (HR Admin) and VP (read-only payroll summaries in granted regions).
- Timesheet lifecycle: Draft, approved and locked, payroll processed. Every edit path refuses a locked week. Only an Admin can reopen one, with a mandatory reason, and it is audited. A week with unresolved meal-break exceptions cannot be approved.
- Secrets live in user secrets or environment variables and are not committed.

## Demo payroll rule

Weekly hours above 40 are paid at 1.5×. This is a demonstration rule only.
