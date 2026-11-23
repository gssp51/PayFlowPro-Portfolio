# Testing

Last verified run: 22 Nov 2026. Both runs are shown live in the technical code walkthrough video (waiting time sped up).

| Suite | Command | Result |
|---|---|---|
| .NET (xUnit) | `dotnet test` | 366 passed, 0 failed (about 16 s) |
| React Native (Jest) | `npx jest` | 5 suites, 48 tests passed (about 9 s) |

## .NET
Covers the payroll calculator (including a 41-hour overtime case), API workflows, shift-window rules, the mobile-kiosk API contract, web controllers, Office timekeeping and authorization. Example security test: a supervisor receives 403 for employees outside their assignment, while an admin receives 200. Tests use an in-memory EF Core database, so no real database is touched.

## Jest
Covers the kiosk flow, API client, signature pad, clock-out screen and app shell, with mocked network calls.

## Note
The source code is private, so these tests are shown in the videos rather than runnable from this repository.
