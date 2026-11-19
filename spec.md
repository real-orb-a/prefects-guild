# PREFECTS GUILD MANAGEMENT SYSTEM (PGMS)
## Master Development Specification
**Project:** Prefects Guild Management System  
**Version:** 1.0  
**Platform:** Progressive Web Application (PWA)  
**Organisation:** SSLSD  
**Development approach:** Phased development  
**Status:** Master technical and functional specification

---

# 1. PROJECT OVERVIEW

## 1.1 Purpose

PGMS (Prefects Guild Management System) is a comprehensive, centralised web application designed to manage the operations, administration and activities of the school's Prefects Guild.

The system must provide a unified platform for managing:

- Prefect accounts and identities.
- Leadership positions and permissions.
- Duty management.
- General and Senior Prefects rosters.
- Attendance.
- Volunteering and duty replacements.
- Events and event participation.
- Announcements.
- Guild points.
- Analytics and reporting.
- Notifications.
- System administration.
- Audit logging.
- Modular functionality.

The system is intended for use by prefects, Senior Prefects, Head Prefects, authorised teachers, administrators and other approved personnel.

It must be reliable, secure, scalable, responsive and suitable for everyday school operations.

## 1.2 Organisation and campuses

The system operates across two school campuses:

1. Thumama (New Campus).
2. Midmac (Old Campus).

Campus affiliation is an important part of the permission and volunteering system.

Each prefect has one authoritative home campus. This cannot be changed by the prefect themselves. An authorised administrator must perform campus transfers, with the change recorded in the audit log.

## 1.3 Core principles

The system must follow these principles:

1. Centralised identity: one account per person.
2. Permission-based access: access is determined by permissions, not merely by page visibility.
3. Automatic encryption: sensitive information is encrypted server-side without requiring users to manage encryption keys.
4. Comprehensive auditing: all meaningful system activity must be recorded, including small actions.
5. Modular architecture: individual modules can be enabled, disabled and configured independently.
6. Data preservation: disabling a module must never automatically delete its data.
7. Campus separation: campus-specific operational activities must respect campus boundaries.
8. Responsive design: the application must work on desktop computers, laptops, tablets and phones.
9. Deployment flexibility: the application must work locally through XAMPP and be deployable to suitable PHP hosting.
10. No unnecessary paid dependencies.
11. The interface must be professionally designed and feel like a properly developed school management application.
12. The system must never rely exclusively on frontend restrictions for security.

---

# 2. TECHNOLOGY STACK

Use the following technology stack unless a technical limitation makes a particular component impossible.

| Component | Technology |
|---|---|
| Application | Progressive Web Application |
| Frontend | HTML5, CSS3, JavaScript |
| Backend | PHP |
| Database | MySQL / MariaDB |
| Local development | XAMPP |
| Database access | PDO with prepared statements |
| Password hashing | PHP Argon2id |
| Data encryption | Server-side authenticated encryption |
| Authentication | PHP sessions |
| Email | Gmail SMTP |
| Push notifications | Web Push API and VAPID |
| Live volunteering updates | AJAX polling |
| Reporting | Server-generated PDF, Excel and CSV |
| PWA installation | Web App Manifest and Service Worker |

Do not introduce Node.js, Firebase, Supabase, Composer, JWT or paid external services unless explicitly approved.

The application must not require WebSockets.

## 2.1 Frontend requirements

Use clean HTML, CSS and JavaScript.

The frontend must:
- Be responsive.
- Support touchscreen interaction.
- Use reusable components where appropriate.
- Provide consistent navigation.
- Use accessible form controls.
- Display clear loading, success, warning and error states.
- Avoid unnecessary page reloads.
- Validate forms before submission, while retaining all server-side validation.

Do not build the interface using a generic collection of unstyled HTML forms.

The design should be professional, modern, restrained and suitable for an official school administration platform.

Avoid excessive rounded cards, oversized buttons, emoji-heavy navigation and generic template aesthetics.

## 2.2 Backend requirements

All sensitive operations must be handled by PHP.

The backend must:
- Authenticate every protected request.
- Validate every input.
- Verify permissions.
- Verify campus restrictions.
- Perform database operations safely.
- Encrypt sensitive information before storage.
- Decrypt sensitive information only after authorisation.
- Record applicable audit events.
- Return consistent JSON responses for API requests.

The frontend must never be trusted to enforce security.

---

# 3. PROJECT DIRECTORY AND FILE STRUCTURE

## 3.1 Mandatory deployment structure

The front-facing web pages must be located in the project root.

Backend functionality must be organised inside:

- `assets/`
- `api/`
- `config/`

Do not create a separate public directory for the frontend.

The application must be capable of running from a subdirectory, such as:

`http://localhost/PGMS/`

It must not assume that it is installed at the domain root.

## 3.2 Proposed directory structure

PGMS/
│
├── index.php
├── login.php
├── dashboard.php
├── volunteering.php
├── duties.php
├── events.php
├── points.php
├── analytics.php
├── users.php
├── administration.php
├── profile.php
├── logout.php
├── offline.php
│
├── manifest.json
├── service-worker.js
│
├── assets/
│   ├── css/
│   │   ├── main.css
│   │   ├── components.css
│   │   ├── dashboard.css
│   │   ├── tables.css
│   │   └── responsive.css
│   │
│   ├── js/
│   │   ├── app.js
│   │   ├── navigation.js
│   │   ├── dashboard.js
│   │   ├── volunteering.js
│   │   ├── duties.js
│   │   ├── events.js
│   │   ├── points.js
│   │   ├── analytics.js
│   │   └── users.js
│   │
│   ├── images/
│   ├── icons/
│   └── fonts/
│
├── api/
│   ├── auth/
│   ├── system/
│   ├── users/
│   ├── permissions/
│   ├── attendance/
│   ├── duties/
│   ├── rosters/
│   ├── volunteering/
│   ├── events/
│   ├── announcements/
│   ├── points/
│   ├── analytics/
│   ├── notifications/
│   ├── modules/
│   ├── audit/
│   └── shared/
│
└── config/
    ├── app.php
    ├── database.php
    ├── security.php
    ├── encryption.php
    ├── mail.php
    ├── permissions.php
    ├── modules.php
    └── settings.php

Additional files and subdirectories may be introduced when necessary, but the root/frontend and backend separation must be preserved.

## 3.3 CRITICAL: Relative paths only

This is a mandatory project requirement.

**Do not use root-relative paths beginning with `/`.**

The application must use relative paths throughout.

Correct examples:

`../api/system.php`

`assets/css/main.css`

`assets/js/app.js`

`api/auth/login.php`

`config/database.php`

`../assets/images/logo.png`

Incorrect examples:

`/api/system.php`

`/assets/css/main.css`

`/../api/system.php`

`/config/database.php`

Never prepend `/` to a path.

Do not use paths such as `/../api/system.php`. This causes deployment and subdirectory routing problems.

Where different directory depths require different relative paths, calculate the appropriate relative path from the current file.

For JavaScript requests, use relative URLs or a centrally configured relative application base path.

For example:

`fetch('../api/volunteering/list.php')`

Do not hardcode `http://localhost/PGMS/` or a production domain into the application.

## 3.4 Path management

Create a central PHP configuration mechanism for generating relative URLs based on the application's installation location.

This must not generate root-relative paths.

Test the application under both:

- A local installation such as `localhost/PGMS/`.
- A production installation inside a hosting subdirectory.

All links, forms, API calls, CSS, JavaScript, images, service-worker registrations and redirects must work in both environments.

## 3.5 Security of directories

Sensitive configuration files must not be directly accessible through the browser.

Where possible:
- Place configuration files outside the document root.
- If the hosting environment requires `config/` inside the project root, explicitly deny HTTP access to it through server configuration.
- Protect internal backend files against direct unauthorised execution.
- Store uploaded files outside publicly executable locations.
- Never expose encryption keys or SMTP credentials in JavaScript.

The exact deployment protection must be documented for Apache/XAMPP and production hosting.

---

# 4. APPLICATION PAGES

The application must contain the following main pages.

## 4.1 Public landing page — `index.php`

**Access:** Public.

This is the public-facing Prefects Guild website.

Its purpose is to introduce the Guild and provide an entry point into PGMS.

Suggested content:
- Prefects Guild introduction.
- Guild information.
- Public-facing notices.
- Public contact information.
- Relevant school information.
- Prefects System Login button.

The public landing page must not expose:
- Student profiles.
- Private announcements.
- Internal rosters.
- Attendance records.
- Volunteering requests.
- Points.
- Internal administrative information.

The public page and authenticated application must be visually consistent.

## 4.2 Login page — `login.php`

**Access:** Public.

Allow authentication using either:
- School email.
- Admission number.

Both must use the same password-based authentication system.

Include:
- Login form.
- Forgot password.
- Password visibility toggle.
- Appropriate error messages.
- Session protection.
- Login attempt rate limiting.

Only school email addresses ending in `@sslsd.education` are valid for school accounts.

### Password recovery

Implement six-digit email verification.

Workflow:

1. User enters their school email or admission number.
2. Backend identifies the account without revealing whether it exists.
3. A six-digit verification code is sent to the registered school email.
4. User enters the code.
5. Backend verifies the code.
6. The code expires after approximately ten minutes.
7. After successful verification, the user sets a new password.
8. The new password is securely hashed.
9. Previous reset tokens are invalidated.
10. Existing sessions are revoked according to the security policy.

Additional requirements:
- Cryptographically secure code generation.
- Store a protected hash of the code, not the plaintext code.
- One-time use.
- Maximum verification attempts.
- Rate limiting.
- Invalidate older codes when a new code is issued.
- Audit all relevant recovery activity.

Never email a user's password.

## 4.3 Dashboard — `dashboard.php`

**Access:** All authenticated users.

The dashboard must be dynamically generated based on:
- Prefect category.
- Assigned roles.
- Permissions.
- Campus.
- Enabled modules.
- Core settings.

Different categories may have different dashboards.

Suggested information includes:
- Welcome message.
- Upcoming duties.
- Current assignments.
- Pending replacement requests.
- Relevant events.
- Important announcements.
- Personal activity.
- Management shortcuts where authorised.

Head Prefects may receive additional operational information based on their permissions.

### Points visibility

Points must be controlled through central Core Settings.

If points are enabled and visible to the user:
- Show their current balance.
- Optionally show recent transactions.
- Provide access to Points Management if permitted.

If points are hidden:
- Do not show a points card.
- Do not display a points label.
- Do not display an empty points section.
- Do not leave a points placeholder.

Hiding points must not stop point calculations or delete historical records.

## 4.4 Volunteering — `volunteering.php`

**Access:** Authenticated users with the appropriate module permissions.

The page allows prefects to find and claim available duty replacements.

Features:
- Available opportunities.
- Duty type.
- Date and time.
- Campus.
- Relevant duty details.
- Claim button.
- Direct replacement requests.
- Incoming requests.
- Sent requests.
- Replacement history.

Only display opportunities from the user's home campus to ordinary prefects.

The system must enforce campus restrictions on the backend.

### Live updates

Use five-second AJAX polling.

Do not use WebSockets.

Requirements:
- Poll only while the volunteering page is open and visible.
- Fetch only changes since the previous update cursor.
- Update affected entries without refreshing the page.
- Remove claimed opportunities after the next successful update.
- Insert newly available opportunities.
- Pause polling when the browser tab is hidden or offline.
- Resume with a delta update when visible again.

The database is authoritative. A displayed opportunity must be revalidated when a claim is submitted.

## 4.5 Duties and Roster — `duties.php`

**Access:** Authenticated users.

This page combines personal duty management, replacement requests and published rosters.

Organise it into three sections.

### A. My Duties

Show:
- Upcoming assignments.
- Previous assignments.
- Duty type.
- Date.
- Start and end time.
- Campus.
- Location.
- Completion status.
- Replacement status.
- Relevant instructions.

Users can declare unavailability for an individual duty assignment.

### B. Volunteer Requests

Show:
- Incoming replacement requests.
- Outgoing replacement requests.
- Pending requests.
- Accepted requests.
- Declined requests.
- Relevant replacement history.

Users can accept or decline requests they are authorised to respond to.

### C. Full Roster

Show:
- General Prefects Roster.
- Senior Prefects Roster.
- Monthly calendar.
- List view.
- Date filtering.
- Duty filtering.
- Search, where permitted.

All prefects may view published rosters from both campuses.

Viewing another campus's published roster does not permit cross-campus volunteering.

Management controls must only appear for authorised users.

## 4.6 Events — `events.php`

**Access:** Authenticated users, subject to permissions.

The Events page must be permission-aware.

Ordinary prefects can:
- View relevant events.
- View event information.
- Respond to volunteer requests.
- Accept or decline participation requests.
- View confirmed allocations.
- View previous participation.

Authorised leadership can:
- Create events.
- Edit events.
- Configure positions.
- Specify required prefect numbers.
- Set eligibility.
- Select specific prefects.
- Send requests.
- Review responses.
- Allocate positions.
- Close and archive events.

## 4.7 Points Management — `points.php`

**Access:** Conditional on the points module and visibility settings.

Prefects can:
- View their balance.
- View their transaction history.
- See transaction reasons where permitted.

Authorised personnel can:
- Search for prefects.
- View balances.
- View transaction histories.
- Add points.
- Deduct points.
- Select predefined reasons.
- Enter custom reasons.
- View adjustment histories.

The points page and its navigation entry must disappear when the module or relevant visibility setting is disabled.

## 4.8 Analytics — `analytics.php`

**Access:** Year 13+ accounts with the appropriate permissions.

Analytics must support:
- Individual prefect reports.
- Attendance statistics.
- Duty completion.
- Volunteering.
- Replacements.
- Event participation.
- Points analytics, if enabled.
- Campus comparisons.
- General and Senior roster statistics.
- Historical reporting.

Filters:
- Individual prefect.
- Campus.
- Category.
- Duty.
- Date.
- Week.
- Month.
- Year.
- Academic year.
- Event.

Export:
- PDF.
- Excel.
- CSV.
- Print-friendly reports.

Year 13+ status is a minimum eligibility restriction, not automatic unrestricted access.

## 4.9 Users — `users.php`

**Access:** Year 13+ accounts with appropriate permissions.

Features:
- Search by name.
- Search by admission number.
- Filter by campus.
- Filter by category.
- View authorised profile information.
- Create accounts.
- Edit accounts.
- Suspend accounts.
- Archive accounts.
- Assign roles.
- Assign leadership positions.
- Perform bulk imports.

Viewing users and modifying accounts must be separate permissions.

## 4.10 Administration — `administration.php`

**Access:** Year 13+ accounts with appropriate permissions.

The administration page contains operational configuration.

Suggested sections:
- Duty configuration.
- Roster configuration.
- Attendance configuration.
- Physical/digital processing settings.
- Points settings.
- Event settings.
- Announcement settings.
- Campus settings.
- Leadership configuration.
- Email templates.
- Audit log access.
- General system settings.

### Module Control restriction

Module installation, activation, deactivation, removal, dependencies and migrations must be available exclusively to the System Administrator role.

Year 13+ status or ordinary Administration access must never grant Module Control access.

---

# 5. NAVIGATION AND ACCESS CONTROL

The application must have one consistent navigation system.

Suggested navigation:

MAIN
- Dashboard
- Volunteering
- Duties and Roster
- Events

MANAGEMENT
- Points Management
- Analytics
- Users
- Administration

ACCOUNT
- Profile
- Logout

Navigation entries must be dynamically generated.

Rules:
1. Do not display inaccessible pages.
2. Do not display disabled modules.
3. Do not display Points Management when points visibility is disabled for the user.
4. Do not expose administration controls without permission.
5. Never rely on hiding navigation entries as the only security measure.
6. Direct access to a restricted URL must be rejected by the backend.
7. All APIs must perform their own authorisation checks.

---

# 6. IDENTITY AND ACCOUNT MANAGEMENT

## 6.1 Account structure

Every prefect must have one central account.

Store the following information where applicable:

- Full name.
- School email.
- Admission number.
- Class.
- Campus.
- Prefect category.
- Leadership positions.
- Account status.
- Creation date.
- Last login.
- Guild history.

Categories:

| Category | Minimum year |
|---|---|
| Trainee Prefect | Year 10+ |
| Junior Prefect | Year 11+ |
| Prefect | Year 12+ |
| Senior Prefect | Year 13+ |
| Head Prefect | Year 13+ |

Eligibility rules must be configurable.

Do not permanently hardcode school year/category eligibility into application logic.

## 6.2 Leadership positions

Head Prefect positions must be configurable.

Each position can have:
- Name.
- Description.
- Campus scope.
- Assigned account.
- Start date.
- End date.
- Active status.
- Permissions.

Campus scope:
- Thumama.
- Midmac.
- Both.

Multiple positions may be assigned where permitted.

A Both-campus position grants only the permissions explicitly assigned to that position. It does not automatically grant ordinary cross-campus volunteering rights.

## 6.3 Account operations

Support:
- Individual account creation.
- Editing.
- Suspension.
- Archival.
- Password reset.
- Account recovery.
- Session revocation.
- Bulk CSV import.
- Bulk spreadsheet import.

Account changes must be audited.

Do not permanently delete accounts when historical operational records depend on them. Prefer archival and deactivation.

---

# 7. PERMISSIONS AND AUTHORISATION

Implement granular role-based access control.

Possible roles include:
- System Administrator.
- Authorised Teacher.
- TIC.
- Head Prefect.
- Senior Prefect.
- Prefect.
- Other configurable roles.

Users may have multiple roles.

Permissions must be independently assignable.

Examples:
- View own profile.
- View other prefect profiles.
- Edit profiles.
- Create accounts.
- Suspend accounts.
- View attendance.
- Process physical attendance.
- Process digital attendance.
- Correct attendance.
- View duty rosters.
- Create duties.
- Edit duties.
- Publish rosters.
- Manage replacements.
- View confidential reasons.
- Manage events.
- Publish announcements.
- View points.
- Adjust points.
- View analytics.
- Export reports.
- View audit logs.
- Configure system settings.
- Manage modules.

## 7.1 Scope restrictions

Permissions must support scope:
- Own account.
- Assigned campus.
- Both campuses.
- Specific operational area.
- System-wide.

The backend must evaluate both permission and scope.

A user must never gain access to information simply because they can guess an API endpoint or record identifier.

---

# 8. SECURITY AND ENCRYPTION

Security must be implemented from Phase 1.

## 8.1 Passwords

Use PHP's `password_hash()` and `password_verify()` with Argon2id.

Requirements:
- Unique salt per password, handled by PHP.
- Configurable secure hashing parameters.
- Password rehashing when parameters change.
- No plaintext password storage.
- No reversible password encryption.
- Login rate limiting.
- Secure session handling.

## 8.2 Automatic field-level encryption

Sensitive data must be encrypted automatically on the server before being written to the database.

Users must not be required to enter an encryption key.

The backend must decrypt data only after checking the relevant permissions.

Sensitive information includes:
- Student names where appropriate.
- Admission numbers.
- School email addresses.
- Phone numbers, if collected.
- Confidential reasons.
- Attendance information.
- Duty and replacement records where sensitive.
- Signature stroke data.
- Other designated private information.

Public content such as published public announcements does not need field-level encryption.

## 8.3 Encryption implementation

Use authenticated encryption, preferably AES-256-GCM through PHP OpenSSL, with:
- Unique nonce/IV for each encryption operation.
- Authentication tag.
- Key version identifier.
- Separate keys for encryption and blind indexes.
- Secure key management.
- Key rotation support.

Encryption keys must:
- Never be stored in ordinary database tables.
- Never be exposed to the browser.
- Never be committed to source control.
- Be stored outside publicly accessible directories.
- Be backed up securely.

The application must support encryption key versioning.

## 8.4 Searching encrypted data

Some encrypted fields need to be searchable.

Use keyed HMAC blind indexes for fields such as admission number and email.

Use separate encryption and HMAC keys.

Normalised search values must be used consistently.

Never store a plaintext duplicate simply to make encrypted data searchable.

## 8.5 Important security clarification

This is server-side encryption, not end-to-end encryption.

The server must decrypt information to perform authorised operations. Therefore, protecting the server, configuration files and encryption keys is essential.

Do not claim that encrypted data is inaccessible to the server itself.

## 8.6 General security

Implement:
- HTTPS in production.
- Secure, HttpOnly and SameSite session cookies.
- Session ID regeneration after login.
- CSRF protection.
- XSS prevention.
- SQL injection prevention.
- PDO prepared statements.
- Output escaping.
- Input validation.
- File upload validation.
- Login rate limiting.
- Secure error handling.
- Permission checks before decryption.
- Audit logging for sensitive data access.

Never return encryption keys, SMTP passwords, internal configuration or stack traces to the browser.

---

# 9. COMPREHENSIVE AUDIT LOGGING

The audit system is one of the most important parts of PGMS.

It must record all meaningful activity, including small actions.

Implement a central audit service in Phase 1. Every subsequent module must use it.

## 9.1 Events to record

### Authentication
- Login attempts.
- Successful logins.
- Failed logins.
- Logout.
- Password reset requests.
- Verification attempts.
- Password changes.
- Session revocation.
- Account recovery.

### User management
- Account creation.
- Profile viewing where sensitive.
- Profile editing.
- Account suspension.
- Account archival.
- Campus transfer.
- Role assignment.
- Permission changes.
- Leadership position changes.

### System administration
- Configuration views where sensitive.
- Configuration changes.
- Security setting changes.
- Module installation.
- Module activation.
- Module deactivation.
- Module removal.
- Migration activity.

### Operational activity
- Duty creation and editing.
- Roster creation and publication.
- Attendance processing.
- Attendance correction.
- Unavailability submission.
- Volunteer claims.
- Replacement requests.
- Request acceptance or rejection.
- Duty completion.
- Event creation and changes.
- Announcement publication.
- Points additions and deductions.

### Sensitive access
- Access to confidential information.
- Exports.
- Bulk operations.
- Access-denied events.
- Failed authorisation checks.

## 9.2 Audit record fields

Each audit record should include:

- Unique event ID.
- Actor account ID.
- Action.
- Module.
- Affected record ID.
- Timestamp.
- Success/failure.
- Relevant previous values.
- Relevant new values.
- IP address where available.
- Session reference.
- Device/browser information where appropriate.
- Reason, where applicable.

Do not store passwords, session tokens, encryption keys, plaintext secrets or unnecessary decrypted sensitive data in logs.

## 9.3 Integrity

Audit logs must be append-only for ordinary users.

Do not allow users to edit or delete historical audit records through normal application interfaces.

Corrections must create new audit records.

Implement tamper-evident protection, such as chained record hashes or another documented integrity mechanism.

Access to audit logs must be permission-controlled.

---

# 10. MODULAR ARCHITECTURE AND MODULE MANAGER

The application must use a modular architecture.

Modules must be independently registered and controlled.

Planned modules:
- Identity and Authentication.
- Attendance.
- Duty Management.
- Rosters.
- Volunteering.
- Events.
- Announcements.
- Points.
- Analytics.
- Notifications.

## 10.1 Module manifests

Each module should have a `module.json` manifest.

It should describe:
- Module ID.
- Name.
- Version.
- Description.
- Category.
- Enabled status.
- Dependencies.
- Required permissions.
- Navigation entries.
- Configuration fields.
- Database migrations.
- Compatibility information.

A manifest describes module metadata. It must not execute arbitrary code.

## 10.2 Module Manager functionality

System Administrators must be able to:
- Discover available modules.
- Validate module manifests.
- Register modules.
- Enable modules.
- Disable modules.
- Configure modules.
- Check dependencies.
- Check compatibility.
- View module health.
- Review errors.
- Track versions.
- Run approved migrations.
- Archive or remove modules where safe.

Disabling a module must not delete its data.

Before removing a module, check:
- Dependencies.
- Existing records.
- Data retention requirements.
- Whether other modules rely on its services.

Only the System Administrator can access Module Control.

---

# 11. PHASE 1 — CORE FOUNDATION

**Phase 1 scope:**
1. Public landing page.
2. Login and identity.
3. Dashboard foundation.
4. Permissions.
5. Encryption.
6. Audit logging.
7. Module Manager.

This phase establishes the foundation for every later feature.

## 11.1 Public landing page

Implement the complete public landing page, responsive design and login entry point.

## 11.2 Identity

Implement:
- Account creation.
- Login.
- Logout.
- Password hashing.
- Password recovery.
- Account management.
- School email validation.
- Admission number login.
- Session management.
- Profile structure.
- Campus assignment.
- Prefect category assignment.
- Leadership position structure.

## 11.3 Permissions

Implement:
- Roles.
- Permissions.
- Role-permission assignments.
- User-role assignments.
- Campus scopes.
- Permission checking service.
- Dynamic navigation.
- Page-level protection.
- API-level protection.

## 11.4 Encryption

Implement:
- Key management.
- Authenticated encryption.
- Encrypted database fields.
- Blind indexes.
- Key versioning.
- Decryption authorisation.
- Secure configuration storage.

## 11.5 Audit

Implement the central audit service and connect every Phase 1 feature to it.

## 11.6 Module Manager

Implement:
- Module registration.
- Manifest validation.
- Enable/disable.
- Dependency checks.
- Module configuration.
- System Administrator restriction.

## 11.7 Phase 1 acceptance criteria

Do not move to Phase 2 until:
- Public landing page works.
- Login works.
- Password recovery works.
- Accounts can be created and managed.
- Permissions work.
- Campus restrictions are enforced.
- Sensitive fields are encrypted.
- Authorised data can be retrieved and decrypted.
- Audit events are recorded.
- Module registration works.
- Module enable/disable works without deleting data.
- The application works from a subdirectory.
- All paths follow the relative-path requirement.
- Security checks are tested.

---

# 12. PHASE 2 — DUTY MANAGEMENT, ROSTERS AND ATTENDANCE

**Phase 2 scope:**
1. Duty Management.
2. General and Senior Rosters.
3. Volunteering and replacements.
4. Attendance.
5. Physical and digital processing.
6. Duty completion and history.

## 12.1 Duty Management

Authorised users must be able to configure duty types.

Each duty type must support:
- Name.
- Description.
- Campus.
- Location.
- Eligible categories.
- Required prefect count.
- Frequency.
- Sessions.
- Start and end times.
- Replacement rules.
- Signature requirements.
- Roster type.
- Active/inactive status.

Examples include:
- Class Duty.
- Gate Duty.
- Corridor Duty.
- Assembly Duty.
- Other configurable duties.

A duty may have multiple independent sessions.

For example, Class Duty can have:
- Before School.
- After School.

Unavailability for one session must not automatically affect another session.

## 12.2 Duty assignments

Each assignment must have a permanent unique identifier.

Store:
- Original assigned prefect.
- Actual performer.
- Duty type.
- Date.
- Start time.
- End time.
- Campus.
- Session.
- Assignment status.
- Completion status.
- Replacement history.
- Processing method.
- Processing user.
- Completion timestamp.

The original assignee must remain identifiable even after replacement.

## 12.3 Physical and digital duty processing

These methods must be independently configurable.

Example:
- Physical duty processing: ON.
- Digital duty processing: OFF.

### Physical processing

An authorised person checks the physical duty register and records completion in PGMS.

Record:
- Processing user.
- Actual performer.
- Original assignee.
- Time.
- Duty assignment.
- Source method.

### Digital processing

The prefect signs on a touchscreen to verify completion where configured.

Store the signature against the specific duty transaction.

The system must not use a permanent account-level signature.

### External replacement arrangements

The system must support cases where prefects arrange a replacement outside PGMS.

Example:
- Ahmed was originally assigned.
- Sara actually performed the duty.
- An authorised processor records Sara as the actual performer.
- Ahmed remains the original assignee.
- The record identifies the source as an unrecorded external arrangement.

Do not fabricate a volunteer request or claim that the replacement occurred through the application.

## 12.4 Attendance

Attendance must support both physical and digital processing.

### Physical attendance

A prefect signs the physical attendance register.

An authorised processor records the attendance in PGMS.

### Digital attendance

A prefect identifies themselves and signs on a touchscreen.

The signature is associated with that attendance transaction.

### Independent configuration

Physical attendance and digital attendance must each have their own enable/disable setting.

For example:
- Physical attendance: ON.
- Digital attendance: ON.
- Physical duty processing: ON.
- Digital duty processing: OFF.

When a method is disabled:
- Do not allow new records through that method.
- Preserve all historical records.
- Keep historical processing methods visible.

### Attendance records

Store:
- Prefect.
- Date.
- Session.
- Status.
- Processing user.
- Processing timestamp.
- Source method.
- Correction history.
- Relevant signature reference.

### Processing permissions

Only authorised personnel may process or correct attendance.

Eligible permission groups include:
- Head Prefects with the relevant permission.
- TICs.
- Authorised Disciplinary Committee teachers.

Ordinary prefects must not process or modify attendance records.

### Corrections

Corrections must:
- Require a reason.
- Preserve the original record.
- Record who made the correction.
- Record when it occurred.
- Create an audit event.

Do not overwrite historical information without retaining its history.

## 12.5 Digital signature stroke data

Store the original vector stroke data rather than relying exclusively on a PNG image.

Each signature may contain:
- X and Y coordinates.
- Point timestamps.
- Stroke start/end.
- Stroke order.
- Pressure, if supported.
- Canvas dimensions.
- Coordinate scaling.
- Capture timestamp.
- Associated transaction ID.
- User identity and verification context.

Encrypt signature data automatically on the server before database storage.

Preserve the original stroke data.

Generate a visual rendering or preview when required.

Do not implement fingerprint recognition in the current project scope. Keep the architecture open for future integration.

## 12.6 General and Senior Prefects Rosters

Provide two roster categories:
- General Prefects Roster.
- Senior Prefects Roster.

Both should use the same underlying roster engine.

They may have separate:
- Eligibility rules.
- Permissions.
- Duties.
- Analytics.
- Management settings.

Support shared duties where explicitly configured.

### Monthly roster builder

Authorised users must be able to:

1. Select General or Senior.
2. Select academic year.
3. Select month.
4. Select duty type.
5. Select duty sessions and dates.
6. Search for a prefect by name or admission number.
7. Select a prefect from autocomplete.
8. Retrieve their official account details.
9. Check eligibility.
10. Check overlapping assignments.
11. Check capacity.
12. Review the roster.
13. Publish it.

Roster publication must create a versioned record.

Published roster history must be preserved.

All prefects can view published rosters from both campuses.

## 12.7 Volunteering and replacements

Volunteering is driven by existing duty assignments.

A prefect may declare unavailability for a particular assignment.

They may provide a reason.

The reason can be marked confidential.

Once submitted:
- The relevant slot becomes available.
- The opportunity appears on the appropriate campus volunteering board.
- Other eligible prefects can claim it.
- The first successful claim wins.

### Claims

The database must prevent two prefects from claiming the same available slot.

Use database transactions and appropriate locking.

When claimed:
- Remove the opportunity from the available board.
- Record the replacement.
- Preserve the original assignment.
- Preserve the replacement history.

### Direct replacement requests

A prefect may enter another prefect's admission number.

The server must:
- Resolve the account.
- Check campus.
- Check eligibility.
- Check overlapping duties.
- Create a pending request.
- Notify the recipient through the application.

The recipient may accept or decline.

The replacement is only transferred after acceptance.

The behaviour following a decline must be configurable.

### Confidential reasons

Confidential unavailability reasons must only be visible to authorised leadership.

An authorised Head may remove the confidentiality flag where permitted.

Preserve:
- Original reason.
- Original confidentiality status.
- Who changed it.
- When it changed.
- Why it changed.

### Campus isolation

This is a strict requirement.

Thumama prefects:
- See Thumama volunteering opportunities.
- Claim Thumama opportunities.
- Send and receive Thumama replacement requests.

Midmac prefects:
- See Midmac volunteering opportunities.
- Claim Midmac opportunities.
- Send and receive Midmac replacement requests.

Ordinary prefects must not:
- See the other campus's volunteering requests.
- Claim the other campus's duties.
- Request replacements from the other campus.
- Receive cross-campus replacement requests.

Published rosters remain visible across both campuses.

A Both-campus Head Prefect position may provide authorised oversight, but does not automatically remove ordinary campus restrictions.

Enforce all these rules in database queries and backend services.

## 12.8 Overlapping duty prevention

No prefect may have overlapping assignments.

This applies to:
- Initial roster creation.
- Volunteering claims.
- Receiving direct requests.
- Accepting direct requests.
- Roster changes.
- Administrative assignments.

For example:
- Duty A: 07:00–07:30.
- Duty B: 07:15–07:45.

The system must reject the second assignment.

A different non-overlapping time is allowed.

Check conflicts using actual date/time intervals.

Perform the final check on the server inside an atomic database transaction.

Never rely only on frontend checks.

## 12.9 Five-second volunteering polling

Do not implement WebSockets.

The volunteering board must use lightweight AJAX polling every five seconds.

Requirements:
- Delta updates only.
- No full-page reload.
- No full-board refetch every five seconds.
- Pause when the tab is hidden.
- Resume when visible.
- Pause when offline.
- Update only affected cards.
- Database remains authoritative.

## 12.10 Phase 2 acceptance criteria

- Duty types can be configured.
- Multiple sessions are supported.
- General and Senior rosters work.
- Monthly rosters can be built and published.
- Published versions are preserved.
- Overlapping assignments are prevented.
- Campus restrictions are enforced.
- Volunteering claims are race-safe.
- Direct replacement requests work.
- Physical attendance processing works.
- Digital attendance processing works when enabled.
- Physical duty processing works.
- Digital duty processing works when enabled.
- Signatures are stored as encrypted transaction-specific stroke data.
- Corrections preserve history.
- All relevant activity is audited.

---

# 13. PHASE 3 — EVENTS, ANNOUNCEMENTS AND POINTS

**Phase 3 scope:**
1. Event Management.
2. Announcements.
3. Guild Points Management System.

## 13.1 Event Management

Events must support:
- Event name.
- Description.
- Date.
- Campus.
- Venue.
- Start time.
- End time.
- Required positions.
- Number of prefects required per position.
- Eligible categories.
- Application deadline.
- Specific prefect requests.
- Volunteer responses.
- Confirmed allocations.
- Event status.
- Historical records.

### Event workflow

1. Authorised user creates an event.
2. Positions and requirements are configured.
3. Specific prefects may be selected or requests sent to eligible groups.
4. Prefects receive the request.
5. Prefects accept or decline.
6. Leadership reviews responses.
7. Final allocations are made.
8. Confirmed participants are recorded.
9. Event is completed and archived.

Interest alone must not count as confirmed participation.

Actual participation and completion must be recorded separately where appropriate.

## 13.2 Announcements

Support three announcement priorities:

- Normal.
- Important.
- Urgent.

Each announcement must support:
- Title.
- Content.
- Priority.
- Target audience.
- Campus targeting.
- Prefect category targeting.
- Publication date.
- Expiry date.
- Attachments where permitted.
- Author.
- Publisher.
- Publication status.
- Version history.

Support:
- Drafts.
- Scheduled publication.
- Publication.
- Archival.
- Editing with history.

Only publishing an announcement should trigger publication notifications.

Saving a draft must not notify users.

## 13.3 Guild Points Management System

The Points System is an integrated Core service.

It must be fast and easy to use.

### Points account

Each prefect must have an individual points account.

The account should provide:
- Current balance.
- Transaction history.
- Additions.
- Deductions.
- Reasons.
- Dates.
- Linked activities.

### Automatic points

Support configurable points for:
- Completing an assigned duty.
- Volunteering for another prefect's duty.
- Event participation.
- Other configurable Guild contributions.

The exact point values and reasons will be configured later.

Do not invent permanent award values.

Point-award rules must be configurable by authorised personnel.

### Award triggers

Automatic awards must be linked to verified activities.

Recommended default:
- Award completion-related points after verified completion, not simply when a duty is claimed.
- Do not award duplicate points for the same qualifying activity.
- Keep the exact trigger configurable.

The system should distinguish between:
- Claiming a duty.
- Accepting a replacement.
- Actually completing the duty.

These are different events.

### Manual point management

Authorised users must be able to add or deduct points at any time.

The adjustment process should be quick:

1. Search for a prefect.
2. Select the prefect.
3. Choose addition or deduction.
4. Enter the amount.
5. Select a predefined reason or enter a custom reason.
6. Confirm the adjustment.

Record:
- Prefect.
- Amount.
- Transaction type.
- Reason.
- Actor.
- Timestamp.
- Linked activity, if applicable.
- Adjustment reference.

### Reasons

Authorised users must be able to:
- Add reasons.
- Edit active reason descriptions.
- Deactivate reasons.
- Create new reasons.

Do not delete historical reasons used in previous transactions.

Preserve the historical reason text for each transaction.

### Ledger design

Use an immutable transaction ledger.

Do not rely on a single editable balance field as the authoritative record.

Balance should be calculated from the ledger or maintained as a derived balance that is reconciled against the ledger.

Example:

| Activity | Change |
|---|---:|
| Duty completion | +5 |
| Volunteering | +10 |
| Manual reward | +15 |
| Manual deduction | -5 |

These values are illustrative only.

A deduction must never erase the original earned transaction.

Mistakes must be corrected using reversal or adjustment transactions.

### Duplicate prevention

Each automatic award must have a unique source activity reference or idempotency key.

A qualifying activity must not award points twice.

### Configuration

Support:
- Points enabled/disabled.
- Automatic awards enabled/disabled.
- Duty completion points.
- Volunteering points.
- Event participation points.
- Different values for different duty types.
- Activity exclusions.
- Manual adjustments enabled/disabled.
- Roles allowed to adjust.
- Points visibility for prefects.
- Points visibility for leadership.
- Negative balance policy or zero-floor policy.

Changing an award rule must not retroactively change historical transactions.

### Points visibility

Points visibility must be configurable centrally.

If disabled for a user:
- Hide the points dashboard card.
- Hide points navigation.
- Hide points balance.
- Hide points history.
- Hide points references elsewhere in the interface.

Hiding points must not disable accounting or automatic awards.

### Points and other modules

Duty Management, Volunteering and Events must call the central Points service.

Do not implement separate points calculation systems inside each module.

---

# 14. PHASE 4 — ANALYTICS AND NOTIFICATIONS

**Phase 4 scope:**
1. Analytics and reporting.
2. Push notifications.
3. Email notifications.
4. Notification templates and delivery management.

## 14.1 Analytics

Provide comprehensive analytics with permission-aware access.

### Individual prefect analytics

Show:
- Attendance count.
- Attendance percentage.
- Duties assigned.
- Duties completed.
- Missed or unverified duties.
- Voluntary replacements received.
- Duties covered for others.
- Unavailability.
- Direct replacement requests.
- Event participation.
- Monthly history.
- Points history and totals, if enabled.

### Duty analytics

Show:
- Number of assignments.
- Completion count.
- Replacements.
- Volunteer claims.
- Unfilled slots.
- Required prefect count.
- Monthly frequency.
- Historical trends.

### Filters

Support:
- Individual.
- Duty.
- Date.
- Week.
- Month.
- Year.
- Academic year.
- Campus.
- Prefect category.
- Event.

### Reports

Allow authorised users to:
- Generate PDF reports.
- Generate Excel reports.
- Generate CSV reports.
- Print reports.

All exports must respect the same permissions and confidentiality rules as normal page access.

Analytics must be descriptive and informative.

Do not automatically treat low statistics as misconduct or impose disciplinary decisions.

## 14.2 Notifications

Use:
- Web Push.
- Service Worker.
- Push API.
- Notifications API.
- VAPID.
- Gmail SMTP.

Support multiple registered devices per user.

Push permission must be requested through an appropriate user-initiated interaction.

For iOS/iPadOS, explain that Web Push requires a supported version and Home Screen installation of the PWA.

### Exact notification rules

Do not expand notifications beyond the following triggers without approval.

| Trigger | Push | Email |
|---|---|---|
| General roster published | Yes | Yes |
| Senior roster published | Yes | Yes |
| Someone volunteers for unavailable duty | Yes | No |
| Direct volunteer request received | Yes | No |
| New event volunteer request sent | Yes | Yes |
| Normal announcement published | Yes | Yes |
| Important announcement published | Yes | Yes |
| Urgent announcement published | Yes | Yes |
| Attendance recorded | No | No |
| Duty completed | No | No |
| Unavailability submitted | No | No |

Phase 1 password recovery emails remain necessary and are not considered general notification functionality.

### Notification delivery

Implement:
- Notification queue.
- Retry handling.
- Delivery status.
- Failed delivery records.
- Device subscription management.
- Expired subscription cleanup.
- Duplicate notification prevention.
- Notification history.

### Email templates

Provide editable templates for:

1. Duty Roster Published.
2. Event Volunteer Request.
3. Normal Announcement.
4. Important Announcement.
5. Urgent Announcement.

Each template must support:
- Subject.
- HTML body.
- Plain-text alternative.
- Enabled/disabled.
- Variables.
- Preview.
- Test email.
- Last edited by.
- Last edited date.
- Version history.

Suggested variables:

`{{recipient_name}}`

`{{prefect_category}}`

`{{campus}}`

`{{roster_name}}`

`{{roster_month}}`

`{{event_name}}`

`{{event_date}}`

`{{event_venue}}`

`{{announcement_title}}`

`{{announcement_content}}`

`{{announcement_priority}}`

`{{published_by}}`

`{{system_url}}`

Email credentials must remain server-side.

---

# 15. DATABASE ARCHITECTURE

Use MySQL/MariaDB.

Use InnoDB tables, foreign keys where appropriate, indexes and appropriate data types.

Use database migrations rather than manually modifying production schemas.

The following are logical table groups. Exact names and normalisation may be refined during implementation.

## 15.1 Identity

- users
- prefect_profiles
- prefect_categories
- campuses
- account_statuses

## 15.2 Leadership and permissions

- roles
- permissions
- role_permissions
- user_roles
- leadership_positions
- user_positions
- permission_scopes

## 15.3 Authentication and sessions

- password_reset_requests
- login_attempts
- user_sessions
- account_verification

## 15.4 Audit

- audit_logs
- audit_integrity_checkpoints

## 15.5 Modules

- modules
- module_settings
- module_dependencies
- module_migrations

## 15.6 Attendance

- attendance_sessions
- attendance_records
- attendance_corrections

## 15.7 Digital signatures

- signature_records
- signature_strokes

## 15.8 Duties

- duty_types
- duty_sessions
- duty_assignments
- duty_completions
- duty_corrections

## 15.9 Rosters

- rosters
- roster_entries
- roster_versions

## 15.10 Volunteering

- unavailability_records
- volunteer_offers
- volunteer_requests
- replacement_history

## 15.11 Events

- events
- event_positions
- event_volunteers
- event_allocations

## 15.12 Announcements

- announcements
- announcement_targets
- announcement_attachments
- announcement_versions

## 15.13 Points

- points_accounts
- points_transactions
- points_reasons
- points_rules
- points_visibility_settings

## 15.14 Notifications

- notifications
- push_subscriptions
- notification_queue
- email_templates
- email_queue
- email_delivery_logs

## 15.15 System

- system_settings
- encryption_key_versions
- application_errors

Use appropriate relationships and indexes.

Do not store unnecessary duplicate copies of sensitive information.

Historical records must remain interpretable even when users, roles, duties or configuration entries change.

---

# 16. PWA REQUIREMENTS

PGMS must be installable as a Progressive Web App.

Implement:
- Web App Manifest.
- Service Worker.
- Installability.
- Application icons.
- Appropriate theme colours.
- Offline fallback page.
- Cache management.
- Versioned static assets.

## 16.1 Offline behaviour

The application must not pretend that sensitive operations succeeded while offline.

The following may be cached where safe:
- Static CSS.
- JavaScript.
- Public landing page resources.
- Non-sensitive application shell.
- A page when logged in (fallback page) showing they are not connected to the internet and check and retry

The following must require a successful server connection:
- Login.
- Attendance processing.
- Duty completion.
- Volunteering claims.
- Replacement acceptance.
- Points adjustments.
- Account changes.
- Administration operations.

Never cache decrypted confidential records in an uncontrolled manner.

## 16.2 Responsive design

Support:
- Desktop.
- Laptop.
- Tablet.
- Mobile.
- Touchscreen PCs.

The interface must remain usable at smaller screen sizes.

---

# 17. GENERAL SYSTEM-WIDE REQUIREMENTS

## 17.1 Data integrity

- Use database transactions for critical operations.
- Prevent duplicate records.
- Preserve historical records.
- Use unique identifiers.
- Validate foreign-key relationships.
- Prevent race conditions.
- Use atomic operations for claims and assignments.

## 17.2 Error handling

Provide:
- Consistent API response formats.
- User-friendly error messages.
- Server-side error logging.
- No exposed stack traces.
- Appropriate HTTP status codes.

## 17.3 API design

Organise PHP endpoints by module.

Use consistent JSON responses.

Example response structure:

{
  "success": true,
  "message": "Operation completed successfully.",
  "data": {}
}

Failure responses should use an appropriate error code and safe message.

Do not expose internal SQL errors or sensitive configuration.

## 17.4 Performance

- Use pagination.
- Index commonly searched fields.
- Avoid loading entire rosters unnecessarily.
- Use delta polling for volunteering.
- Avoid unnecessary database queries.
- Cache safe configuration data where appropriate.
- Avoid repeated decryption of the same data within a request.

## 17.5 Accessibility

- Keyboard navigation.
- Readable contrast.
- Clear labels.
- Accessible form errors.
- Appropriate touch target sizes.
- Screen-reader-friendly semantic HTML.

---

# 18. DEVELOPMENT AND IMPLEMENTATION RULES FOR THE AI CODING AGENT

These instructions are mandatory.

## 18.1 Work in phases

Build only the currently authorised phase.

Do not begin implementing later-phase functionality prematurely.

However, design the database and architecture so future phases can integrate without major restructuring.

Complete and test each phase before moving to the next.

## 18.2 Do not make assumptions

When a significant policy or workflow is unspecified:
- Identify the missing decision.
- Explain the implications.
- Ask one clear question when necessary.

Do not silently invent point values, school policies, eligibility rules or disciplinary rules.

Use configurable settings where appropriate.

## 18.3 Do not use fake functionality

Do not create buttons that do nothing.

Do not create placeholder APIs that pretend to perform operations.

Do not simulate successful database operations.

If a feature is not implemented, clearly identify it as unavailable.

## 18.4 Security first

Every API endpoint must:
- Validate authentication.
- Validate permissions.
- Validate scope.
- Validate input.
- Apply CSRF protection where applicable.
- Use prepared statements.
- Record relevant audit events.

Do not rely on JavaScript for access control.

## 18.5 Relative paths

This rule must be followed in every generated file.

The frontend lives in the root.

Backend files live in `assets/`, `api/` and `config/`.

**Never use a leading slash in application paths.**

Use relative paths such as:

`../api/system.php`

`assets/css/main.css`

`../assets/images/logo.png`

Never use:

`/api/system.php`

`/assets/css/main.css`

`/../api/system.php`

Test subdirectory deployment.

## 18.6 Code quality

- Use readable and maintainable PHP.
- Separate business logic from presentation.
- Reuse shared services.
- Avoid duplicating permission logic.
- Avoid enormous single PHP files.
- Use prepared statements.
- Use clear function and variable names.
- Document non-obvious security logic.
- Keep frontend and backend responsibilities separate.

## 18.7 Database changes

Every schema change must have a migration.

Do not silently drop tables or columns.

Do not reset production data.

Do not use destructive migrations without explicit approval and a documented backup procedure.

## 18.8 Testing

Each phase must have:
- Functional tests.
- Permission tests.
- Security tests.
- Database integrity tests.
- Responsive UI tests.
- Deployment/path tests.

Test both authorised and unauthorised users.

Test both campuses.

Test concurrent operations where relevant.

## 18.9 Progress reporting

At the end of each development task, report:
- Files created.
- Files modified.
- Features implemented.
- Database changes.
- Security considerations.
- Tests performed.
- Known limitations.
- Remaining work.

Do not claim that a feature is complete unless its backend, frontend, database and permission handling are actually implemented.

---

# 19. PHASE COMPLETION CHECKLIST

## Phase 1 — Foundation

- [ ] Public landing page.
- [ ] Login.
- [ ] Dashboard foundation.
- [ ] Identity and accounts.
- [ ] Password recovery.
- [ ] Roles and permissions.
- [ ] Campus scopes.
- [ ] Encryption.
- [ ] Blind indexes.
- [ ] Audit logging.
- [ ] Module Manager.
- [ ] PWA foundation.
- [ ] Relative-path deployment testing.
- [ ] Security testing.

## Phase 2 — Daily operations

- [ ] Duty configuration.
- [ ] Duty assignments.
- [ ] General roster.
- [ ] Senior roster.
- [ ] Monthly roster builder.
- [ ] Roster publication and versioning.
- [ ] Volunteering board.
- [ ] Five-second polling.
- [ ] Direct replacement requests.
- [ ] Replacement history.
- [ ] Campus isolation.
- [ ] Overlapping duty prevention.
- [ ] Physical attendance.
- [ ] Digital attendance.
- [ ] Physical duty processing.
- [ ] Digital duty processing.
- [ ] Signature stroke storage.
- [ ] Corrections and audit history.

## Phase 3 — Guild activities

- [ ] Event management.
- [ ] Event volunteer requests.
- [ ] Event allocations.
- [ ] Announcements.
- [ ] Announcement targeting.
- [ ] Points accounts.
- [ ] Points ledger.
- [ ] Automatic points.
- [ ] Manual adjustments.
- [ ] Points reasons.
- [ ] Points visibility settings.
- [ ] Points permissions.
- [ ] Duplicate award prevention.

## Phase 4 — Insights and communication

- [ ] Individual analytics.
- [ ] Duty analytics.
- [ ] Attendance analytics.
- [ ] Volunteering analytics.
- [ ] Event analytics.
- [ ] Points analytics.
- [ ] Report exports.
- [ ] Push notifications.
- [ ] Email notifications.
- [ ] Notification queue.
- [ ] Email templates.
- [ ] Delivery logs.
- [ ] Retry handling.

---

# 20. FINAL DEVELOPMENT DIRECTIVE

Build PGMS as a complete, production-minded Prefects Guild Management System, not as a simple school website or a collection of disconnected CRUD pages.

The system must have a coherent architecture, a consistent interface, strong permission enforcement, reliable data handling, comprehensive audit logging and automatic server-side encryption.

The four phases are authoritative:

**PHASE 1:** Public landing page, identity, permissions, encryption, audit logs and Module Manager.

**PHASE 2:** Duty Management, General and Senior Rosters, Volunteering, replacements and Attendance.

**PHASE 3:** Events, Announcements and the Guild Points Management System.

**PHASE 4:** Analytics and Notifications.

Do not skip phases, introduce unapproved technologies or remove requirements to simplify implementation.

Prioritise correctness, security, maintainability and a polished user experience over rushing to produce a large quantity of code.

The result must be a system that can realistically be used to operate the Prefects Guild across both school campuses.
