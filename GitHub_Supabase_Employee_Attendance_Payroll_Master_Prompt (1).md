# GITHUB + SUPABASE DEVELOPMENT MASTER PROMPT
## Employee Management, Employee Number Attendance, DTR, Overtime, Holiday Pay and Payroll System

**Project type:** Production-ready web application  
**Repository:** Existing GitHub repository (inspect it before changing anything)  
**Frontend:** React + TypeScript + Vite  
**Backend / Database:** Supabase (PostgreSQL)  
**Authentication:** Supabase Auth  
**File storage:** Supabase Storage  
**Server-side privileged operations:** Supabase Edge Functions and PostgreSQL functions  
**Hosting:** Prepare for deployment on Vercel, Netlify, or another compatible static frontend host  
**Reporting:** Excel, CSV, and PDF. Google Sheets integration is optional and must be configured separately.  
**Timezone:** Asia/Manila  
**Currency:** Philippine Peso (₱)

---

# MASTER INSTRUCTIONS

Build and maintain this application directly in the existing GitHub repository. Do not use Google AI Studio, Firebase, Firestore, or Google Cloud as the application backend.

The goal is to deliver a real, working business application—not a static mockup or a collection of disconnected screens.

## Non-negotiable rules

1. Inspect the existing GitHub repository, source code, package files, routes, components, database integrations, and current functionality before editing.
2. Preserve the existing working UI, features, and records wherever possible.
3. Do not delete, overwrite, reset, or replace existing production data.
4. Do not replace real project configuration with placeholder credentials.
5. Use Supabase as the single primary source of truth for application data.
6. Do not put the Supabase `service_role` key or other privileged secrets in frontend code, GitHub commits, or public environment variables.
7. Every form, button, report, dashboard card, and action must perform its real function.
8. Use Asia/Manila for attendance, schedules, DTR, holidays, and payroll date calculations.
9. Use Philippine Peso for monetary values.
10. Use database constraints, transactions, Row Level Security (RLS), and server-side validation to protect data.
11. Never rely only on hiding frontend buttons for security.
12. Implement in phases. Do not claim a feature is complete until it has been implemented and tested.
13. Before changing database structures, inspect existing tables and data. Use additive, versioned SQL migrations wherever possible.
14. Commit and push completed work to the connected GitHub repository only after tests pass. Do not force-push or overwrite unrelated user changes.

---

# PHASE 1 — GITHUB PROJECT INSPECTION AND SETUP

## 1.1 Inspect the repository

Before making changes:

- Inspect the full repository structure.
- Identify the framework, build tool, package manager, and current entry points.
- Review `package.json` and lockfile.
- Review existing routes, pages, components, hooks, services, and styles.
- Identify current authentication and database integrations.
- Identify existing environment variable names.
- Identify existing tests and build scripts.
- Check for existing employee, attendance, and payroll data structures.
- Identify incomplete features and known errors.

Do not replace the existing project with a new starter template unless the repository is genuinely empty and the user authorizes it.

## 1.2 Git workflow

- Work on the existing branch unless a feature branch is appropriate.
- Check `git status` before editing.
- Do not discard uncommitted user changes.
- Make focused, understandable changes.
- Keep secrets out of commits.
- Add or update `.gitignore`.
- Add a useful `README.md`.
- Commit changes with descriptive messages.
- Push to the connected GitHub remote after successful validation.
- If GitHub authentication or remote access is unavailable, finish the local changes and clearly explain what the user must do to push.

## 1.3 Recommended project structure

Adapt this to the existing repository rather than forcing a disruptive restructure.

```text
project/
├── public/
├── src/
│   ├── app/
│   ├── components/
│   ├── features/
│   │   ├── auth/
│   │   ├── employees/
│   │   ├── attendance/
│   │   ├── kiosk/
│   │   ├── schedules/
│   │   ├── reports/
│   │   ├── salary/
│   │   ├── overtime/
│   │   ├── holidays/
│   │   ├── payroll/
│   │   ├── leave/
│   │   ├── users/
│   │   ├── settings/
│   │   └── audit/
│   ├── hooks/
│   ├── lib/
│   │   ├── supabase/
│   │   ├── date/
│   │   └── validation/
│   ├── services/
│   ├── types/
│   └── styles/
├── supabase/
│   ├── migrations/
│   ├── functions/
│   └── seed.sql
├── tests/
├── .env.example
├── .gitignore
├── README.md
└── package.json
```

---

# PHASE 2 — SUPABASE PROJECT CONFIGURATION

Connect the existing application to the user's Supabase project.

## 2.1 Required Supabase services

Use:

- Supabase PostgreSQL database.
- Supabase Auth.
- Supabase Storage.
- PostgreSQL Row Level Security (RLS).
- PostgreSQL constraints, indexes, and functions.
- Supabase Edge Functions for privileged operations and integrations where appropriate.
- Supabase Realtime for live dashboard updates where appropriate.

Do not use Firebase or Firestore.

## 2.2 Environment variables

Use environment variables. For a Vite frontend, the public configuration may use:

```env
VITE_SUPABASE_URL=
VITE_SUPABASE_ANON_KEY=
```

If using a publishable key supported by the Supabase project, use the correct current public key format.

Privileged server-side secrets must only exist in secure server/Edge Function secrets, never in `VITE_` variables:

```env
SUPABASE_SERVICE_ROLE_KEY=
```

Do not commit `.env` files. Commit only `.env.example` with empty values and clear setup instructions.

## 2.3 Supabase client

Create one reusable Supabase client in the existing project's appropriate library folder.

- Use the official Supabase JavaScript client.
- Handle missing environment variables clearly.
- Do not create multiple competing clients.
- Centralize authentication and database access.
- Use typed database definitions where practical.
- Handle loading, empty, success, and error states.

## 2.4 SQL migrations

Create versioned SQL migration files in `supabase/migrations/`.

Migrations must:

- Be ordered and repeatable where practical.
- Prefer additive changes.
- Avoid destructive table drops or resets.
- Preserve existing records.
- Include foreign keys, unique constraints, check constraints, and indexes.
- Enable RLS on every table containing application data.
- Define explicit policies for each role and data category.
- Include database functions/triggers only when appropriate.
- Be documented in the README.

Do not merely describe the schema. Create the actual SQL migration files in the repository.

## 2.5 Required database entities

Create or adapt the following relational tables. Use UUID primary keys unless an existing project convention requires otherwise.

- `profiles` — Supabase user profile, role, employee link, account status.
- `employees` — employee master information.
- `employee_salary_history` — effective-dated salary history.
- `schedules` — reusable work schedules.
- `employee_schedules` — employee schedule assignments and effective dates.
- `attendance` — daily attendance summary.
- `attendance_events` — immutable scan/event history.
- `attendance_corrections` — original and corrected attendance values.
- `overtime_requests` — employee overtime requests and approval records.
- `leave_requests` — leave requests and decisions.
- `holidays` — configured Philippine holidays.
- `payroll_periods` — payroll period definitions and status.
- `payroll_records` — employee payroll snapshots.
- `payroll_adjustments` — authorized additions and deductions.
- `company_settings` — company and attendance configuration.
- `audit_logs` — traceable user actions.
- `qr_credentials` — hashed or securely stored QR credential references and revocation state.
- `integration_sync_queue` — optional queue for Google Sheets or other integrations.

Use appropriate relationships, indexes, and unique constraints. For example, prevent more than one attendance summary for the same employee and work date, and prevent duplicate payroll records for the same employee and payroll period.

Sensitive salary and payroll fields must not be exposed to employees or unauthorized roles.

---

# PHASE 3 — AUTHENTICATION, FIRST ADMIN, AND ROLE ACCESS

Create a professional login page with:

- Email address.
- Password.
- Sign in.
- Forgot password.
- Sign out.

Use Supabase Auth for authentication.

## 3.1 Roles

Support four roles:

### SUPER ADMIN

Full system access. Can:

- Register and edit employees.
- Edit salary and salary history.
- Generate and revoke QR credentials.
- Create Admin and HR accounts.
- Manage attendance and corrections.
- Approve or reject overtime.
- Manage payroll.
- Manage holidays and rest days.
- Manage reports.
- Configure integrations.
- View audit logs.
- Manage company settings.

### ADMIN

Can access assigned modules and permissions:

- View dashboard.
- Manage authorized employee records.
- Edit salary only if explicitly authorized.
- Monitor and correct attendance.
- Approve or reject overtime.
- Manage payroll only if authorized.
- Generate reports.

Cannot create Super Admin accounts or change their own role.

### HR

Can:

- View employee records.
- Monitor attendance.
- Manage authorized employee information.
- Manage leave requests.
- Generate attendance reports.

Salary and payroll access must be separately authorized.

### EMPLOYEE

Can:

- View their own profile.
- View their own attendance and DTR.
- Submit overtime requests.
- View overtime approval status.
- View their own payslips.

Employees cannot edit their own salary, attendance, role, or payroll.

## 3.2 First Super Admin provisioning

- Do not hardcode an email address or password.
- Do not permit public registration as Admin or Super Admin.
- Provide a secure first-time provisioning process.
- Use a protected Supabase Edge Function or a documented manual server-side process to assign the first Super Admin.
- Verify that the requesting person is authorized before provisioning.
- Only an existing authorized Super Admin can create additional Admin and HR accounts.
- Never trust a role value submitted by the browser.

## 3.3 Security enforcement

Implement authorization in both:

1. Frontend route and component access.
2. Supabase RLS policies and trusted server-side functions.

Use `auth.uid()` and the trusted profile/role relationship in RLS policies. Prevent users from editing their own role, employee link, salary, or privileged fields.

Do not expose the `service_role` key to the browser.

---

# PHASE 4 — COMPANY SETTINGS AND WORK SCHEDULES

Create a Company Settings page. Super Admin can configure:

- Company name.
- Company logo.
- Company address.
- Contact number.
- Company email.
- Timezone.
- Working days.
- Default schedule.
- Default time in.
- Default time out.
- Lunch break.
- Grace period.
- Rest days.
- Holiday calendar.
- Payroll schedule.
- Overtime rates.
- Attendance rules.

Default schedule:

| Setting | Default |
|---|---|
| Time In | 8:00 AM |
| Time Out | 5:00 PM |
| Lunch Break | 1 hour unpaid |
| Grace Period | 0 minutes |
| Regular Working Hours | 8 hours |
| Timezone | Asia/Manila |

Support individual employee schedules, different rest days, flexible schedules, and overnight shifts.

Store timestamps consistently in PostgreSQL (`timestamptz`) and display/calculate business dates in Asia/Manila. Define a single, tested method for deriving the employee's work date, especially for overnight shifts.

---

# PHASE 5 — EMPLOYEE REGISTRATION AND MANAGEMENT

Create a dedicated **REGISTER EMPLOYEE** action. Only authorized users may register employees.

Registration fields:

1. Employee ID — automatically generated and editable by authorized users.
2. Full name.
3. Employee photo.
4. Position.
5. Department.
6. Contact number.
7. Address.
8. Email.
9. Date hired.
10. Employment type.
11. Employment status.
12. Assigned schedule.
13. Assigned rest day.
14. Salary type.
15. Daily salary.
16. Hourly salary.
17. Monthly salary.
18. Overtime rate.
19. Allowances.
20. Deductions.

On Save:

- Validate required information.
- Check duplicate Employee ID.
- Save the employee to Supabase.
- Upload the employee photo to a private Storage bucket.
- Create a secure QR attendance credential.
- Assign the employee schedule.
- Make the employee available to attendance monitoring.
- Include the employee in applicable reports and payroll.
- Write the action to the audit log.

Employee Management actions:

- View employee.
- Edit employee.
- Upload/change photo.
- Change schedule.
- Change rest day.
- Update salary with effective date and reason.
- Generate/download QR.
- Print employee ID.
- Deactivate employee.
- Restore employee.

Do not permanently delete employee records that have historical attendance or payroll data. Use deactivation/archiving.

---

# PHASE 6 — SUPABASE STORAGE AND EMPLOYEE QR CREDENTIALS

## 6.1 Storage buckets

Create and document appropriate Storage buckets, for example:

- `employee-photos` — private.
- `company-assets` — public only for assets intended to be public.
- `leave-attachments` — private.
- `payslips` — private, if generated files are stored.

Use Storage policies so users can access only files they are authorized to view. Use signed URLs for private files when needed.

## 6.2 QR ID card

Generate a unique QR code for each employee.

Display:

- Company logo.
- Company name.
- Employee photo.
- Employee name.
- Employee ID.
- Position.
- Department.
- QR code.

Allow:

- Download PNG.
- Download PDF.
- Print ID card.
- Preview.
- Revoke and regenerate QR credential.

## 6.3 QR security

- QR content must be a cryptographically random, non-guessable token.
- Do not encode salary, password, home address, or private employee information.
- Store only a secure hash of the raw credential where feasible.
- Validate credentials through a trusted database function or Edge Function.
- Never trust an employee ID supplied by the QR alone.
- Support credential revocation and regeneration.
- Do not expose the full credential in logs or ordinary API responses.

---

# PHASE 7 — QR ATTENDANCE SCANNER

Create a dedicated QR Attendance Scanner page.

Features:

- Camera preview.
- Automatic QR detection.
- Camera permission handling.
- Employee identification.
- Duplicate scan protection.
- Immediate Supabase saving.
- Success and error messages.

Support desktop webcams, laptop cameras, Android rear cameras, and front cameras where needed.

## 7.1 Automatic attendance

- First valid scan for the employee's assigned workday records TIME IN.
- Second valid scan records TIME OUT only when Time In exists and Time Out has not been recorded.
- Never overwrite an existing attendance event.
- Prevent rapid repeated scans and duplicate records using both frontend cooldown and database-level protection.
- If attendance is already complete, display: **“Attendance for this workday has already been completed.”**

Use a PostgreSQL function or trusted Edge Function to validate the QR credential and record the scan atomically. Avoid a frontend sequence that can create duplicate or conflicting records.

## 7.2 Scan confirmation

Show:

- Employee photo.
- Employee name.
- Employee ID.
- Position.
- Department.
- Philippine date and time.
- Time In or Time Out.
- Attendance status.
- Late minutes, when applicable.

Show the result for approximately three seconds, then return to scanner mode.

## 7.3 Error handling

Handle:

- Invalid QR.
- Unknown employee.
- Deactivated employee.
- Revoked QR.
- Duplicate scan.
- Camera permission denied.
- Camera unavailable.
- Network error.
- Existing completed attendance.

Never create attendance for an invalid credential.

---

# PHASE 8 — ATTENDANCE KIOSK / FULL-SCREEN MODE

Create a dedicated **ATTENDANCE KIOSK** button for a company entrance PC, laptop, tablet, or Android device.

Kiosk display:

- Company logo and name.
- Large live clock.
- Philippine date.
- QR camera scanner.
- Scanning instructions.
- Camera status.
- Internet connection status.
- Database connection status.

Kiosk workflow:

1. Initialize the camera after permission is granted.
2. Start QR detection.
3. Wait for employee scans.
4. Validate QR securely.
5. Record attendance through the trusted backend.
6. Display employee confirmation.
7. Return to scanner mode after approximately three seconds.

Buttons:

- **MAXIMIZE** — request browser full-screen mode.
- **MINIMIZE** — exit browser full-screen mode.
- **EXIT KIOSK** — return to the authorized administrative dashboard.

Hide administrative navigation, payroll, salary information, employee management, user management, and private reports.

Employees must not be able to access administrative functions through the kiosk.

Keep the scanner active while the browser/device remains awake. Explain that a normal browser page cannot act as a true operating-system screensaver while the device is locked or sleeping. Provide Windows/Android kiosk and sleep-prevention setup guidance.

---

# PHASE 9 — ATTENDANCE RULES AND CALCULATIONS

Use Asia/Manila and each employee's assigned schedule.

Default Time In: 8:00 AM.

| Actual Time In | Classification |
|---|---|
| Before 8:00 AM | Early Arrival |
| Exactly 8:00 AM | On Time |
| 8:01 AM onward | Late |

Default grace period: zero minutes.

Calculate late minutes from scheduled start time after applying the configured grace period.

Default Time Out: 5:00 PM.

| Actual Time Out | Classification |
|---|---|
| Before 5:00 PM | Early Out / Undertime |
| Exactly 5:00 PM | Regular Time Out |
| After 5:00 PM | Potential Overtime |

Extra time must not automatically become payable overtime.

Calculate:

- Early arrival.
- Late minutes.
- Undertime minutes.
- Regular hours.
- Actual hours.
- Paid hours.
- Approved overtime.
- Attendance status.

Deduct configured unpaid lunch breaks.

Support:

- Different work schedules.
- Overnight shifts.
- Rest days.
- Holidays.
- Approved leave.
- Incomplete attendance.

Early arrival is not automatically payable overtime.

---

# PHASE 10 — LIVE ATTENDANCE DASHBOARD

Create a real-time dashboard using Supabase Realtime or reliable refresh/query invalidation.

Dashboard cards:

- Total Employees.
- Present Today.
- On Time Today.
- Late Today.
- Absent Today.
- On Leave.
- Employees with Overtime.
- Employees Not Yet Timed Out.

Attendance table columns:

- Employee photo.
- Employee ID.
- Employee name.
- Position.
- Department.
- Scheduled Time In.
- Actual Time In.
- Scheduled Time Out.
- Actual Time Out.
- Late minutes.
- Undertime.
- Regular hours.
- Overtime hours.
- Status.
- Estimated daily earnings, visible only to authorized roles.

Status colors:

- Green: Present / On Time.
- Red: Late.
- Orange: Overtime.
- Gray: Absent.
- Blue: Leave.
- Purple: Rest Day / Holiday.

Include date filter, employee search, department filter, status filter, sorting, pagination, export, and print.

Update the dashboard after a successful attendance scan.

---

# PHASE 11 — DAILY, WEEKLY, AND MONTHLY DTR REPORTS

Create separate Daily, Weekly, and Monthly reports.

## Daily report

Display:

- Date.
- Employee ID.
- Employee name.
- Position.
- Department.
- Time In.
- Time Out.
- Late minutes.
- Undertime.
- Regular hours.
- Overtime hours.
- Status.
- Daily salary and estimated earnings, restricted to authorized roles.

Include absent and incomplete attendance records.

## Weekly report

Display:

- Employee name and ID.
- Working days.
- Present days.
- Absent days.
- Leave days.
- Late occurrences.
- Total late minutes.
- Undertime.
- Regular hours.
- Approved overtime.
- Estimated weekly earnings.

## Monthly report

Display:

- Employee name and ID.
- Position and department.
- Working days.
- Present days.
- Absent days.
- Leave days.
- Late occurrences and minutes.
- Undertime.
- Regular hours.
- Approved overtime.
- Rest day work.
- Holiday work.
- Estimated monthly earnings.

All reports must support Excel, CSV, PDF, and Print. Use actual Supabase data and enforce report access permissions.

---

# PHASE 12 — SALARY MANAGEMENT

Support three salary types.

### Daily-paid employees
Calculate eligible payable working days multiplied by the applicable daily rate, with rest day and holiday premiums calculated separately.

### Hourly-paid employees
Calculate eligible paid hours multiplied by the applicable hourly rate.

### Monthly-paid employees
Use the configured monthly compensation method and applicable payroll rules. Do not treat all monthly-paid employees as daily-paid.

Salary fields:

- Daily rate.
- Hourly rate.
- Monthly rate.
- Overtime rate.
- Allowances.
- Meal allowance.
- Transportation allowance.
- Other allowances.
- Deductions.
- Effective date.
- Payroll schedule.

Every salary change must preserve:

- Previous rate.
- New rate.
- Effective date.
- Administrator.
- Reason.
- Timestamp.

Use effective-dated salary history. Updating a current salary must never change an already finalized payroll snapshot.

Salary data must be protected by RLS and available only to authorized users.

---

# PHASE 13 — OVERTIME REQUEST FORM

Add an **OVERTIME REQUEST** action beside the employee attendance/time record form.

Automatically populate:

- Employee ID.
- Employee name.
- Attendance date.
- Scheduled Time In.
- Actual Time In.
- Scheduled Time Out.
- Actual Time Out.

Employee inputs:

- Requested overtime hours.
- Reason.
- YES / NO selection.

If YES:

1. Submit an overtime request.
2. Save it to Supabase.
3. Link it to the attendance record.
4. Set status to PENDING.
5. Display it in Admin Overtime Approval.

YES means the employee is requesting overtime, not that it is approved.

Verify actual overtime from recorded Time Out and the applicable schedule.

If NO:

- Save the NO selection.
- Continue attendance normally.
- Do not generate payable overtime.

Prevent duplicate active overtime requests for the same employee and attendance date unless an authorized administrator permits a revision.

---

# PHASE 14 — OVERTIME APPROVAL MANAGEMENT

Create an Admin Overtime Approval page.

Display:

- Employee name and ID.
- Attendance date.
- Time In.
- Scheduled Time Out.
- Actual Time Out.
- Requested hours.
- Actual verified hours.
- YES / NO.
- Reason.
- Estimated overtime amount.
- Approval status.

Admin actions:

- View details.
- Approve.
- Reject.
- Edit verified hours.
- Add remarks.

Only approved overtime may be included in payroll.

On approval:

- Verify attendance.
- Calculate eligible hours.
- Apply the correct overtime rate.
- Save approved hours and amount.
- Save administrator ID and approval timestamp.
- Make the approved amount available to payroll.

On rejection:

- Save rejection status and reason.
- Do not add overtime pay.

Admin adjustments require a reason and audit log entry.

Employee My Overtime page displays date, requested hours, actual hours, YES/NO, reason, status, approved hours, approved amount, and admin remarks. Employees can access only their own records.

---

# PHASE 15 — SUNDAY, REST DAY, AND HOLIDAY MANAGEMENT

Sunday is not automatically a rest day. Use the employee's assigned rest day.

Use configurable reference multipliers for eligible covered employees:

| Classification | First 8 Hours | Overtime |
|---|---:|---:|
| Ordinary Workday | 100% | 125% |
| Ordinary Rest Day | 130% | 169% |
| Special Non-Working Holiday | 130% | 169% |
| Special Holiday + Rest Day | 150% | 195% |
| Regular Holiday | 200% | 260% |
| Regular Holiday + Rest Day | 260% | 338% |

These are reference defaults, not a universal payroll rule. Keep multipliers configurable and apply applicable Philippine labor regulations, employee coverage, and current official rules. Flag uncertain cases for payroll review.

Holiday eligibility must support:

- Regular holiday pay when not working.
- Required preceding-day attendance, where applicable.
- Approved leave and recognized exceptions.
- Holiday work.
- Rest day coinciding with holiday.
- Employee coverage and exemptions.

Holiday calendar actions:

- Add holidays.
- Edit holidays.
- Delete unprocessed holiday entries.
- Select holiday type.
- Add holiday description.
- Import official Philippine holidays.
- Verify official declarations.

Do not automatically treat unverified future dates as official holidays.

Separate payroll components:

- Regular workday pay.
- Rest day pay.
- Special holiday pay.
- Regular holiday pay.
- Holiday + rest day pay.
- Regular overtime pay.
- Rest day overtime pay.
- Holiday overtime pay.

Save the multiplier actually used in each payroll record.

---

# PHASE 16 — PAYROLL MANAGEMENT

Create a complete payroll processing module.

Statuses:

1. DRAFT.
2. FOR REVIEW.
3. APPROVED.
4. FINALIZED.
5. PAID.

Payroll columns:

- Employee name and ID.
- Department.
- Salary type.
- Payroll period.
- Basic pay.
- Regular workday pay.
- Rest day pay.
- Special holiday pay.
- Regular holiday pay.
- Holiday rest day pay.
- Regular overtime pay.
- Rest day overtime pay.
- Holiday overtime pay.
- Approved overtime hours.
- Allowances.
- Absence deductions.
- Undertime deductions.
- Other deductions.
- Gross pay.
- Net pay.

Formulas:

**Gross Pay = Basic Pay + Eligible Premium Pay + Approved Overtime Pay + Allowances**

**Net Pay = Gross Pay - Applicable Deductions**

Prevent double-counting basic salary and premium amounts.

Use the appropriate salary type, assigned schedule, attendance, approved leave, overtime approval, holiday classification, and configured payroll rules.

Actions:

- Generate payroll.
- Preview payroll.
- Edit authorized adjustments.
- Submit for review.
- Approve payroll.
- Finalize payroll.
- Mark as paid.
- Generate payslip.
- Print.
- Export.

After finalization:

- Lock the payroll snapshot.
- Preserve rates and multipliers used.
- Preserve attendance references.
- Preserve approved overtime references.
- Prevent automatic recalculation.
- Require an authorized correction workflow for adjustments.

Prevent duplicate payroll for the same employee and period using database constraints.

---

# PHASE 17 — EMPLOYEE PAYSLIPS

Create a professional payslip showing:

- Company logo and name.
- Employee name and ID.
- Position and department.
- Payroll period.
- Salary type.
- Basic salary.
- Regular pay.
- Rest day pay.
- Holiday pay.
- Overtime hours and pay.
- Allowances.
- Deductions.
- Gross salary.
- Net salary.
- Payroll date.

Show overtime and holiday pay separately.

Employees may view only their own payslips. Authorized administrators may print and download payslips as PDF.

---

# PHASE 18 — LEAVE MANAGEMENT

Create a Leave Management page.

Employee request fields:

- Employee ID.
- Employee name.
- Leave type.
- Start date.
- End date.
- Number of days.
- Reason.
- Attachment, when required.

Admin/HR actions:

- Approve.
- Reject.
- Add remarks.
- View leave history.

Approved leave must update attendance monitoring and payroll according to the configured leave policy. Do not mark approved leave as absence.

Store private attachments in Supabase Storage with appropriate RLS-backed access policies or signed URLs.

---

# PHASE 19 — ATTENDANCE CORRECTIONS

Authorized administrators can correct:

- Missing Time In.
- Missing Time Out.
- Incorrect Time In.
- Incorrect Time Out.
- Incorrect attendance classification.

Every correction requires:

- Employee.
- Attendance date.
- Original value.
- Corrected value.
- Reason.
- Administrator.
- Timestamp.

Never erase the original attendance event or correction history. Store the correction as a traceable record and update the attendance summary through a trusted operation.

---

# PHASE 20 — OPTIONAL GOOGLE SHEETS INTEGRATION

Supabase remains the primary database.

If Google Sheets integration is enabled, suggested tabs are:

1. Employees.
2. Attendance Logs.
3. Daily Reports.
4. Weekly Reports.
5. Monthly Reports.
6. Salary Records.
7. Payroll.
8. Overtime Requests.
9. Holiday Records.
10. Company Settings.
11. Audit Logs.

Synchronization process:

1. Save the record to Supabase first.
2. Create a synchronization task.
3. Send the record to the correct Google Sheet through a secure server-side integration.
4. Confirm successful synchronization.
5. Retry failed synchronization.
6. Update sync status.

Prevent duplicate spreadsheet rows using stable record IDs/idempotency keys.

Google Sheets failure must never delete or invalidate successfully saved Supabase records. Do not put Google credentials or service account secrets in frontend code.

If Google Sheets credentials are not configured, the rest of the application must continue working.

---

# PHASE 21 — AUDIT LOGS

Create a dedicated Audit Logs page.

Record:

- Employee registration and edits.
- Salary changes.
- QR credential changes.
- Attendance scans.
- Attendance corrections.
- Overtime submissions, approvals, and rejections.
- Payroll generation, approval, and finalization.
- Holiday changes.
- User account changes.
- Company settings changes.
- Integration sync errors.

Each log should include:

- User ID.
- User role at the time of action, where appropriate.
- Action.
- Record ID.
- Previous value.
- New value.
- Reason.
- Timestamp.

Restrict access to authorized administrators. Audit logs must not be editable by ordinary users. Prefer server-side insertion for sensitive events.

---

# PHASE 22 — SIDEBAR NAVIGATION BY ROLE

## SUPER ADMIN

1. Dashboard.
2. QR Attendance Scanner.
3. Attendance Kiosk.
4. Register Employee.
5. Employee Management.
6. Employee QR Generator.
7. Attendance Monitoring.
8. Daily Reports.
9. Weekly Reports.
10. Monthly Reports.
11. Salary Management.
12. Payroll Management.
13. Overtime Approval.
14. Holiday & Rest Day Management.
15. Attendance Corrections.
16. Leave Management.
17. User Accounts.
18. Integrations.
19. Audit Logs.
20. Company Settings.

## ADMIN

1. Dashboard.
2. Employee Management.
3. Attendance Monitoring.
4. Attendance Corrections.
5. Overtime Approval.
6. Holiday Management, if authorized.
7. Salary Management, if authorized.
8. Reports.
9. Payroll, if authorized.

## HR

1. Dashboard.
2. Employee Records.
3. Attendance Monitoring.
4. Leave Management.
5. Reports.

## EMPLOYEE

1. My Attendance.
2. My Time Records.
3. My Overtime.
4. My Payslips.
5. My Profile.

Display only pages the user is permitted to access. Enforce the same restrictions in RLS and server-side operations.

---

# PHASE 23 — UI DESIGN AND RESPONSIVENESS

Create a modern professional dashboard:

- Clean white and light-gray background.
- Professional blue accent.
- Clear typography.
- Responsive sidebar.
- Modern dashboard cards.
- Easy-to-read tables.
- Clear status badges.
- Touch-friendly mobile controls.
- Professional forms.
- Confirmation dialogs.
- Toast notifications.
- Loading, empty, and error states.

Support Windows desktop, laptops, Android phones, Android tablets, and modern browsers.

Dashboard charts:

- Daily attendance.
- Monthly attendance trends.
- Late attendance trends.
- Department attendance.
- Overtime summary.
- Payroll summary.

Keep the layout clean and avoid unnecessary visual clutter.

---

# PHASE 24 — TESTING AND VALIDATION

Add tests for business logic and important workflows. Run lint, type checks, tests, and production build.

## Supabase/database tests

- Connect to the configured Supabase project.
- Create an employee.
- Save employee data.
- Refresh the application.
- Verify the record persists.
- Verify existing records remain intact.
- Verify RLS blocks unauthorized reads and writes.
- Verify employees cannot alter their own role, salary, attendance, or payroll.

## QR tests

- Generate employee QR.
- Scan Time In.
- Scan Time Out.
- Verify attendance.
- Try duplicate scanning.
- Verify duplicate protection.
- Revoke credential and verify it no longer works.

## Time calculation tests

Test:

- 7:59 AM.
- 8:00 AM.
- 8:01 AM.
- 4:59 PM.
- 5:00 PM.
- 5:01 PM.

Verify attendance classifications, grace period behavior, lunch deductions, and overnight shifts.

## Overtime tests

- Submit YES.
- Verify pending request.
- Approve overtime.
- Verify approved hours.
- Verify overtime amount in payroll.
- Submit NO.
- Verify no overtime pay is generated.
- Verify unauthorized users cannot approve.

## Holiday tests

Test:

- Ordinary workday.
- Sunday as a working day.
- Sunday as a rest day.
- Weekday rest day.
- Special non-working holiday.
- Regular holiday.
- Holiday falling on a rest day.
- Overtime on a holiday.

Verify configured multipliers and employee eligibility.

## Payroll tests

- Generate payroll.
- Verify basic salary.
- Verify allowances and deductions.
- Verify approved overtime.
- Verify rest day and holiday pay.
- Finalize payroll.
- Verify finalized payroll is locked.
- Verify a salary change does not modify historical finalized payroll.

## Kiosk tests

- Open Kiosk.
- Allow camera.
- Scan QR.
- Verify attendance.
- Verify success message.
- Verify scanner returns automatically.
- Test camera errors.
- Verify employee cannot access admin pages.

## Integration tests

If Google Sheets is configured:

- Record attendance.
- Verify Supabase saving.
- Verify Sheets synchronization.
- Test failed sync.
- Retry synchronization.
- Verify no duplicate rows.

Do not report tests as passed unless they were actually run.

---

# PHASE 25 — DEPLOYMENT

Prepare the project for production deployment.

## Supabase setup checklist

1. Create or select the user's Supabase project.
2. Configure the project URL and public key.
3. Apply SQL migrations.
4. Verify tables, foreign keys, indexes, and constraints.
5. Enable and test Supabase Auth.
6. Configure the first Super Admin securely.
7. Configure Storage buckets and policies.
8. Enable and test RLS on all relevant tables.
9. Deploy required Edge Functions.
10. Configure server-side secrets.
11. Test role permissions.
12. Test database persistence.

## GitHub setup checklist

1. Confirm all source files are in the intended repository.
2. Confirm `.env` and secrets are excluded.
3. Include `.env.example`.
4. Include SQL migrations.
5. Include setup and deployment instructions in `README.md`.
6. Run lint, type checks, tests, and production build.
7. Review `git diff` and `git status`.
8. Commit changes with a descriptive message.
9. Push to the intended GitHub branch.
10. Do not force-push or overwrite unrelated work.

## Frontend hosting

Prepare for Vercel, Netlify, or another compatible host.

Configure the required public environment variables in the hosting provider. Ensure camera features are served over HTTPS.

Do not claim deployment is successful until the deployment has actually been completed and verified.

---

# PHASE 26 — IMPLEMENTATION ORDER

Implement in dependency order:

1. Inspect GitHub repository and current application.
2. Establish safe Git workflow and project architecture.
3. Configure Supabase connection.
4. Create SQL migrations and database schema.
5. Configure RLS, constraints, and indexes.
6. Implement authentication and role permissions.
7. Implement first Super Admin provisioning.
8. Implement company settings and schedules.
9. Implement employee registration and management.
10. Implement Supabase Storage.
11. Implement secure QR generation and ID cards.
12. Implement QR attendance scanner.
13. Implement attendance rules and DTR calculations.
14. Implement attendance monitoring dashboard.
15. Implement daily, weekly, and monthly reports.
16. Implement salary management and history.
17. Implement overtime requests and approvals.
18. Implement Sunday, rest day, and holiday management.
19. Implement payroll computation and payslips.
20. Implement kiosk and full-screen mode.
21. Implement leave management and attendance corrections.
22. Implement optional Google Sheets integration.
23. Implement audit logs and security review.
24. Run full testing.
25. Build and prepare production deployment.
26. Commit and push verified changes to GitHub.

Do not skip dependencies. If the project is too large for one development pass, implement it phase by phase while preserving existing code and records.

---

# REQUIRED DELIVERABLES IN THE GITHUB REPOSITORY

Deliver:

- Working React + TypeScript application.
- Supabase client configuration.
- SQL migration files under `supabase/migrations/`.
- RLS policies for every protected table.
- Supabase Edge Functions or trusted database functions for privileged workflows.
- Storage bucket setup and policies.
- Authentication and role-based access.
- Employee registration and management.
- Employee QR credentials and ID cards.
- QR attendance scanner.
- Attendance Kiosk / Full-screen mode.
- Attendance dashboard.
- Daily, weekly, and monthly DTR.
- Late and undertime calculations.
- Overtime request and approval.
- Sunday, rest day, and holiday pay configuration.
- Salary history.
- Payroll generation, approval, finalization, and payslips.
- Leave management.
- Attendance corrections.
- Optional Google Sheets synchronization.
- Audit logs.
- Responsive desktop and Android interface.
- `.env.example`.
- `.gitignore`.
- Complete `README.md` with GitHub, Supabase, local development, migration, first-admin, and deployment instructions.
- Tests for critical business logic and security-sensitive operations.

## Final development report

At the end, provide a truthful report listing:

- Features completed.
- Features partially completed.
- Features not started.
- SQL migrations created and whether they were applied.
- RLS policies created and tested.
- Edge Functions created and deployed, if applicable.
- Tests actually run and their results.
- Build result.
- Git commit hash and push status, if available.
- Supabase settings or credentials the user still needs to configure.
- Deployment status and remaining steps.

**MOST IMPORTANT:** All modules must use the same Supabase employee records, schedules, attendance events, salary history, overtime approvals, holiday calendar, and payroll records. Preserve existing data, prevent duplicate records, enforce access control in the database, and make payroll calculations traceable.

Build a real, usable business application—not just a dashboard mockup.
