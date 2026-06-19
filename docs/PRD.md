# PRD — OpsPilot

> **Product Name:** OpsPilot  
> **Product Type:** RPA Operations, Ticketing, Monitoring, and Reporting Dashboard  
> **Backend Stack:** Go  
> **Frontend Stack:** Next.js / React.js  
> **Primary Goal:** Membantu tim RPA / Automation melakukan tracking pekerjaan, monitoring robot, eskalasi issue, monitoring license/asset, dan generate report operasional harian, bulanan, maupun tahunan.

---

## 1. Ringkasan Produk

OpsPilot adalah aplikasi dashboard internal untuk membantu tim RPA / Automation dalam mengelola pekerjaan operasional sehari-hari. Sistem ini menggabungkan fungsi **ticketing**, **robot monitoring**, **issue escalation**, **license monitoring**, dan **reporting** dalam satu platform.

Masalah utama yang ingin diselesaikan:

1. Issue robot sering tersebar di chat pribadi, WhatsApp, Teams, atau email.
2. Progress pekerjaan sulit dilacak secara historis.
3. Eskalasi ke user, infra, network, vendor, atau tim lain tidak terdokumentasi rapi.
4. Monitoring robot masih manual atau belum terpusat.
5. License, account robot, server, dan dependency aplikasi sering tidak termonitor sampai terjadi issue.
6. Report harian, bulanan, dan tahunan masih dibuat manual.
7. Root cause issue berulang tidak terdokumentasi sebagai knowledge base.

OpsPilot bertujuan menjadi **single source of truth** untuk pekerjaan RPA operation.

---

## 2. Visi Produk

Menjadi dashboard operasional yang membantu tim automation memonitor robot, menyelesaikan issue lebih cepat, menjaga SLA, dan menghasilkan report operasional dengan mudah.

Tagline sementara:

> **OpsPilot — Your automation operations command center.**

---

## 3. Target Pengguna

### 3.1 Admin

Admin mengelola konfigurasi sistem, user, role, master data, dan pengaturan umum.

### 3.2 RPA Developer / RPA Support

Tim yang bertanggung jawab melakukan monitoring robot, handling issue, update ticket, investigasi root cause, dan eskalasi.

### 3.3 Business User / Requester

User bisnis yang membuat ticket, melaporkan issue, request automation baru, atau memberi feedback terhadap ticket.

### 3.4 Manager / Lead

User yang membutuhkan dashboard summary, SLA monitoring, dan report untuk kebutuhan operasional atau management.

---

## 4. Goals

### 4.1 Product Goals

1. Menyediakan sistem ticketing untuk issue, request, enhancement, CR, incident, support, dan license.
2. Menyediakan dashboard monitoring status robot dan pekerjaan RPA.
3. Menyediakan timeline eskalasi dan discussion log pada setiap ticket.
4. Menyediakan report daily, monthly, yearly berbasis data ticket dan robot run.
5. Menyediakan license dan asset monitoring agar expired date dapat dipantau.
6. Menyediakan knowledge base root cause dan troubleshooting.
7. Menjadi portfolio project yang real-world dan relevan dengan pekerjaan Backend Engineer + RPA Automation.

### 4.2 Engineering Goals

1. Backend menggunakan Go dengan struktur yang clean dan scalable.
2. Frontend menggunakan Next.js / React.js dengan layout dashboard modern.
3. API dibuat rapi dan konsisten.
4. Database schema mudah dikembangkan.
5. Sistem dibuat modular agar fitur bisa dikembangkan bertahap.
6. MVP harus bisa berjalan lokal dengan mudah menggunakan Docker Compose.

---

## 5. Non-Goals Untuk MVP

Fitur berikut **tidak wajib dibuat pada MVP pertama**:

1. Integrasi langsung ke UiPath Orchestrator API.
2. Integrasi langsung ke Microsoft Teams / WhatsApp.
3. Real-time websocket monitoring.
4. Multi-tenant company complex.
5. Approval workflow kompleks.
6. AI auto root cause analysis.
7. Mobile app native.
8. SSO enterprise.

Fitur di atas boleh disiapkan secara struktur, tetapi jangan menjadi blocker untuk MVP.

---

## 6. MVP Scope

MVP OpsPilot harus memiliki fitur utama berikut:

1. Authentication sederhana.
2. Role-based access basic.
3. Dashboard summary.
4. Ticket management.
5. Ticket comments / activity timeline.
6. Robot master data.
7. Robot run history manual input / mock input.
8. License & asset monitoring basic.
9. Basic report dan chart.
10. Export report minimal ke CSV.

---

## 7. Modul Utama

## 7.1 Authentication & Authorization

### Objective

User harus login sebelum mengakses dashboard.

### Features

1. Login page.
2. Logout.
3. Auth session / token.
4. Role user.
5. Protected routes di frontend.
6. Middleware auth di backend.

### Roles

| Role | Description |
|---|---|
| `ADMIN` | Full access ke semua fitur dan setting. |
| `SUPPORT` | Bisa handle ticket, robot monitoring, escalation, report. |
| `REQUESTER` | Bisa create ticket dan melihat ticket miliknya. |
| `MANAGER` | Bisa melihat dashboard, report, dan detail ticket. |

### MVP Auth Notes

Untuk MVP, boleh menggunakan JWT-based authentication dengan email dan password.

Password harus disimpan dalam bentuk hash, bukan plain text.

---

## 7.2 Dashboard

### Objective

Memberikan overview cepat tentang kondisi operasional RPA.

### Dashboard Widgets

1. Total tickets today.
2. Total open tickets.
3. Total in-progress tickets.
4. Total waiting user tickets.
5. Total resolved tickets this month.
6. SLA breached tickets.
7. Robot success rate.
8. Failed robot runs today.
9. License expiring soon.
10. Monthly ticket trend.

### Chart Requirements

Minimal chart untuk MVP:

1. Ticket by status.
2. Ticket by priority.
3. Ticket by category.
4. Robot run success vs failed.
5. Ticket trend per month.

---

## 7.3 Ticket Management

### Objective

Mencatat dan melacak semua pekerjaan operasional, issue, request, enhancement, dan incident.

### Ticket Types

| Type | Description |
|---|---|
| `ISSUE` | Problem pada robot, aplikasi, data, atau environment. |
| `REQUEST` | Permintaan baru dari user. |
| `ENHANCEMENT` | Improvement pada automation existing. |
| `CHANGE_REQUEST` | Perubahan logic atau flow existing karena kebutuhan bisnis. |
| `INCIDENT` | Issue production yang berdampak besar. |
| `SUPPORT` | Bantuan manual atau pengecekan data. |
| `LICENSE` | Ticket terkait license, renewal, account, atau akses. |

### Ticket Status

| Status | Description |
|---|---|
| `OPEN` | Ticket baru dibuat. |
| `IN_PROGRESS` | Ticket sedang dikerjakan. |
| `WAITING_USER` | Menunggu response / data dari user. |
| `WAITING_INTERNAL` | Menunggu tim internal seperti infra/network/security. |
| `WAITING_VENDOR` | Menunggu vendor. |
| `RESOLVED` | Issue sudah diselesaikan. |
| `CLOSED` | Ticket sudah ditutup. |
| `CANCELLED` | Ticket dibatalkan. |

### Ticket Priority

| Priority | Description |
|---|---|
| `LOW` | Tidak urgent, dampak kecil. |
| `MEDIUM` | Normal priority. |
| `HIGH` | Berdampak pada operasional penting. |
| `CRITICAL` | Production down atau berdampak besar ke bisnis. |

### Ticket Fields

| Field | Type | Required | Notes |
|---|---|---|---|
| id | UUID / bigint | Yes | Primary key. |
| ticket_no | string | Yes | Format contoh: `OPS-2026-000001`. |
| title | string | Yes | Judul ticket. |
| description | text | Yes | Detail issue/request. |
| type | enum | Yes | Ticket type. |
| category_id | FK | Optional | Category master. |
| priority | enum | Yes | LOW/MEDIUM/HIGH/CRITICAL. |
| status | enum | Yes | Current status. |
| reporter_id | FK users | Yes | User pembuat ticket. |
| assignee_id | FK users | Optional | PIC ticket. |
| department_id | FK | Optional | Department requester. |
| robot_id | FK robots | Optional | Robot terkait. |
| application_id | FK applications | Optional | Aplikasi terkait. |
| sla_due_at | timestamp | Optional | Target SLA. |
| started_at | timestamp | Optional | Waktu mulai dikerjakan. |
| resolved_at | timestamp | Optional | Waktu resolved. |
| closed_at | timestamp | Optional | Waktu closed. |
| root_cause | text | Optional | Root cause akhir. |
| resolution | text | Optional | Solusi akhir. |
| workaround | text | Optional | Workaround sementara. |
| created_at | timestamp | Yes | Audit. |
| updated_at | timestamp | Yes | Audit. |
| deleted_at | timestamp | Optional | Soft delete. |

### Ticket List Requirements

Ticket list harus mendukung:

1. Search by ticket number / title.
2. Filter by status.
3. Filter by type.
4. Filter by priority.
5. Filter by assignee.
6. Filter by robot.
7. Filter by application.
8. Filter by date range.
9. Sort by latest update.
10. Pagination.

### Ticket Detail Requirements

Ticket detail harus menampilkan:

1. Ticket information.
2. Reporter.
3. Assignee.
4. Related robot.
5. Related application.
6. Status and priority.
7. SLA due date.
8. Root cause.
9. Resolution.
10. Timeline activity.
11. Comments / discussion.
12. Attachment list.

---

## 7.4 Ticket Timeline & Comments

### Objective

Mencatat semua update, diskusi, dan eskalasi agar tidak hilang di chat.

### Timeline Event Types

| Event Type | Description |
|---|---|
| `COMMENT` | User menambahkan komentar. |
| `STATUS_CHANGED` | Status ticket berubah. |
| `ASSIGNED` | Ticket di-assign ke PIC. |
| `ESCALATED` | Ticket dieskalasi ke pihak lain. |
| `PRIORITY_CHANGED` | Priority berubah. |
| `ROOT_CAUSE_UPDATED` | Root cause ditambahkan/diubah. |
| `RESOLUTION_UPDATED` | Resolution ditambahkan/diubah. |
| `ATTACHMENT_ADDED` | Attachment ditambahkan. |

### Comment Fields

| Field | Type | Required |
|---|---|---|
| id | UUID / bigint | Yes |
| ticket_id | FK tickets | Yes |
| user_id | FK users | Yes |
| message | text | Yes |
| is_internal | boolean | Yes |
| created_at | timestamp | Yes |

`is_internal = true` berarti komentar hanya untuk tim internal support/admin/manager.

---

## 7.5 RPA Robot Monitoring

### Objective

Menyediakan tempat untuk melihat status robot dan histori run.

### Robot Fields

| Field | Type | Required | Notes |
|---|---|---|---|
| id | UUID / bigint | Yes | Primary key. |
| name | string | Yes | Nama robot. |
| code | string | Yes | Kode unik robot. |
| description | text | Optional | Deskripsi proses. |
| owner_department_id | FK departments | Optional | Owner bisnis. |
| application_id | FK applications | Optional | Aplikasi utama. |
| environment | enum | Yes | DEV/UAT/PROD. |
| status | enum | Yes | ACTIVE/INACTIVE/MAINTENANCE. |
| server_name | string | Optional | Nama server. |
| server_ip | string | Optional | IP server. |
| schedule_info | string | Optional | Jadwal robot. |
| created_at | timestamp | Yes | Audit. |
| updated_at | timestamp | Yes | Audit. |

### Robot Run Status

| Status | Description |
|---|---|
| `RUNNING` | Robot sedang berjalan. |
| `SUCCESS` | Robot berhasil. |
| `FAILED` | Robot gagal. |
| `PARTIAL_SUCCESS` | Sebagian data berhasil. |
| `CANCELLED` | Run dibatalkan. |
| `SKIPPED` | Run dilewati. |

### Robot Run Log Fields

| Field | Type | Required |
|---|---|---|
| id | UUID / bigint | Yes |
| robot_id | FK robots | Yes |
| run_no | string | Optional |
| status | enum | Yes |
| started_at | timestamp | Yes |
| finished_at | timestamp | Optional |
| duration_seconds | integer | Optional |
| total_transaction | integer | Optional |
| success_transaction | integer | Optional |
| failed_transaction | integer | Optional |
| error_message | text | Optional |
| log_summary | text | Optional |
| related_ticket_id | FK tickets | Optional |
| created_at | timestamp | Yes |

### MVP Notes

Untuk MVP, robot run log boleh diinput manual atau menggunakan seed/mock data. Integrasi langsung dengan robot engine boleh dibuat di phase berikutnya.

---

## 7.6 Escalation Management

### Objective

Mencatat eskalasi ticket ke user, internal team, atau vendor.

### Escalation Target Types

| Target Type | Example |
|---|---|
| `USER` | Business user / requester. |
| `INFRA` | Server, VM, resource, access. |
| `NETWORK` | Firewall, whitelist, VPN. |
| `SECURITY` | Account, privilege, policy. |
| `VENDOR` | Third-party application / RPA vendor. |
| `APPLICATION_TEAM` | Owner aplikasi terkait. |

### Escalation Fields

| Field | Type | Required |
|---|---|---|
| id | UUID / bigint | Yes |
| ticket_id | FK tickets | Yes |
| target_type | enum | Yes |
| target_name | string | Yes |
| reason | text | Yes |
| status | enum | Yes |
| escalated_at | timestamp | Yes |
| resolved_at | timestamp | Optional |
| created_by | FK users | Yes |

### Escalation Status

1. `OPEN`
2. `WAITING_RESPONSE`
3. `RESOLVED`
4. `CANCELLED`

---

## 7.7 License & Asset Monitoring

### Objective

Memonitor license, account robot, server, credential, dan dependency agar expiry/issue bisa dicegah.

### Asset Types

| Asset Type | Example |
|---|---|
| `RPA_LICENSE` | UiPath license. |
| `OFFICE_LICENSE` | Office 365 account robot. |
| `SERVER` | RPA PROD server. |
| `BOT_ACCOUNT` | Account aplikasi untuk robot. |
| `VPN` | VPN account/access. |
| `CERTIFICATE` | SSL/API certificate. |
| `SCHEDULER` | Windows Scheduler / Orchestrator schedule. |
| `APPLICATION_ACCESS` | Access aplikasi terkait robot. |

### License / Asset Fields

| Field | Type | Required |
|---|---|---|
| id | UUID / bigint | Yes |
| name | string | Yes |
| type | enum | Yes |
| owner | string | Optional |
| vendor | string | Optional |
| related_robot_id | FK robots | Optional |
| related_application_id | FK applications | Optional |
| start_date | date | Optional |
| expired_date | date | Optional |
| reminder_days | integer | Optional |
| status | enum | Yes |
| notes | text | Optional |
| created_at | timestamp | Yes |
| updated_at | timestamp | Yes |

### Asset Status

1. `ACTIVE`
2. `EXPIRING_SOON`
3. `EXPIRED`
4. `INACTIVE`

---

## 7.8 Reports & Analytics

### Objective

Membantu user membuat laporan monitoring harian, bulanan, dan tahunan.

### Report Types

| Report | Content |
|---|---|
| Daily Monitoring | Ticket hari ini, failed robot, open issue, pending escalation. |
| Monthly Report | Ticket trend, SLA, robot success rate, top issue. |
| Yearly Report | Automation health, issue trend, license summary, workload summary. |
| Robot Report | Run history, success rate, failed transaction. |
| SLA Report | On-time vs breached. |
| License Report | Active, expiring soon, expired. |

### MVP Report Features

1. Filter date range.
2. Generate chart on screen.
3. Export CSV.
4. Summary cards.

### Future Report Features

1. Export PDF.
2. Export Excel with multiple sheets.
3. Scheduled report email.
4. Auto-generate executive summary.

---

## 7.9 Knowledge Base / RCA Library

### Objective

Menyimpan root cause dan solusi issue agar issue berulang bisa lebih cepat ditangani.

### Fields

| Field | Type | Required |
|---|---|---|
| id | UUID / bigint | Yes |
| title | string | Yes |
| issue_pattern | text | Yes |
| root_cause | text | Yes |
| solution | text | Yes |
| workaround | text | Optional |
| related_application_id | FK applications | Optional |
| related_robot_id | FK robots | Optional |
| tags | string[] | Optional |
| created_by | FK users | Yes |
| created_at | timestamp | Yes |
| updated_at | timestamp | Yes |

### MVP Notes

Knowledge base boleh dibuat sederhana dengan CRUD basic.

---

## 7.10 Master Data

Master data yang dibutuhkan:

1. Users.
2. Roles.
3. Departments.
4. Applications.
5. Robots.
6. Ticket categories.
7. SLA rules.
8. Asset types.

---

## 8. Suggested Page Structure

Frontend menggunakan layout dashboard dengan sidebar.

```txt
/login
/dashboard
/tickets
/tickets/create
/tickets/[id]
/robots
/robots/[id]
/robot-runs
/assets
/reports
/knowledge-base
/master/users
/master/departments
/master/applications
/settings
```

---

## 9. UI/UX Requirements

### General UI

1. Clean dashboard style.
2. Sidebar navigation.
3. Topbar with user profile and logout.
4. Responsive layout minimal untuk desktop/tablet.
5. Clear status badge.
6. Table with filter and pagination.
7. Detail page with timeline.
8. Form validation.
9. Empty state for no data.
10. Loading state for API call.

### Recommended Visual Style

OpsPilot sebaiknya punya tampilan:

1. Professional.
2. Clean.
3. Slightly modern/tech.
4. Tidak terlalu ramai.
5. Fokus ke readability data.

### Status Badge Color Direction

| Status | Color Direction |
|---|---|
| Success / Resolved | Green |
| Running / In Progress | Blue |
| Waiting | Yellow / Amber |
| Failed / Critical | Red |
| Closed / Inactive | Gray |

---

## 10. Backend Architecture Direction

Backend menggunakan Go.

Recommended structure:

```txt
backend/
  cmd/
    api/
      main.go
  internal/
    config/
    database/
    middleware/
    module/
      auth/
        handler.go
        service.go
        repository.go
        model.go
        dto.go
      user/
      ticket/
      robot/
      asset/
      report/
      knowledge/
    pkg/
      response/
      validator/
      logger/
      jwt/
      pagination/
  migrations/
  docs/
  go.mod
  go.sum
```

### Backend Principles

1. Pisahkan handler, service, repository.
2. Handler hanya untuk HTTP request/response.
3. Service untuk business logic.
4. Repository untuk database access.
5. Gunakan DTO untuk request/response.
6. Gunakan centralized error response.
7. Gunakan middleware auth untuk protected endpoint.
8. Gunakan pagination standar untuk list endpoint.
9. Gunakan migration untuk database schema.
10. Jangan hardcode config; gunakan environment variables.

### Suggested Go Libraries

Tidak wajib, tetapi boleh digunakan:

1. Router: `chi`, `gin`, atau `fiber`.
2. Database: `pgx` atau `database/sql`.
3. Migration: `golang-migrate`.
4. Validation: `go-playground/validator`.
5. JWT: `golang-jwt/jwt`.
6. Password hash: `bcrypt`.
7. Logger: `zerolog` atau `slog`.

Preferensi untuk project ini:

- Router: `chi` atau `gin`.
- Database: PostgreSQL.
- Migration: `golang-migrate`.
- Logger: `slog` atau `zerolog`.

---

## 11. Frontend Architecture Direction

Frontend menggunakan Next.js / React.js.

Recommended structure:

```txt
frontend/
  app/ or pages/
  components/
    layout/
    ui/
    forms/
    charts/
    tables/
  features/
    auth/
    dashboard/
    tickets/
    robots/
    assets/
    reports/
    knowledge-base/
  lib/
    api.ts
    auth.ts
    utils.ts
  hooks/
  types/
  styles/
  public/
```

### Frontend Principles

1. Gunakan reusable component.
2. Pisahkan page dan feature logic.
3. Semua API call lewat satu API client.
4. Gunakan form validation.
5. Gunakan loading dan error state.
6. Gunakan table component yang reusable.
7. Gunakan chart component yang reusable.
8. Jangan hardcode data kecuali seed/mock untuk MVP.

### Suggested Frontend Libraries

Tidak wajib, tetapi boleh digunakan:

1. UI: Tailwind CSS.
2. Component: shadcn/ui.
3. Form: react-hook-form.
4. Validation: zod.
5. Chart: recharts.
6. Table: TanStack Table.
7. HTTP Client: fetch wrapper atau axios.
8. State: Zustand atau React Context untuk auth.

---

## 12. Database Direction

Database utama: PostgreSQL.

### Core Tables

1. users
2. roles
3. departments
4. applications
5. ticket_categories
6. tickets
7. ticket_comments
8. ticket_activities
9. robots
10. robot_run_logs
11. assets
12. escalations
13. knowledge_base
14. sla_rules

### ERD Concept

```txt
users 1---n tickets as reporter
users 1---n tickets as assignee
users 1---n ticket_comments
users 1---n ticket_activities

departments 1---n users
departments 1---n robots

applications 1---n robots
applications 1---n tickets
applications 1---n assets

robots 1---n robot_run_logs
robots 1---n tickets
robots 1---n assets

tickets 1---n ticket_comments
tickets 1---n ticket_activities
tickets 1---n escalations
tickets 1---n robot_run_logs optional relation
```

---

## 13. API Requirements

All API should use prefix:

```txt
/api/v1
```

### 13.1 Auth API

```txt
POST /api/v1/auth/login
POST /api/v1/auth/logout
GET  /api/v1/auth/me
```

### 13.2 Dashboard API

```txt
GET /api/v1/dashboard/summary
GET /api/v1/dashboard/ticket-chart
GET /api/v1/dashboard/robot-chart
GET /api/v1/dashboard/license-summary
```

### 13.3 Ticket API

```txt
GET    /api/v1/tickets
POST   /api/v1/tickets
GET    /api/v1/tickets/{id}
PUT    /api/v1/tickets/{id}
DELETE /api/v1/tickets/{id}
PATCH  /api/v1/tickets/{id}/status
PATCH  /api/v1/tickets/{id}/assign
POST   /api/v1/tickets/{id}/comments
GET    /api/v1/tickets/{id}/activities
POST   /api/v1/tickets/{id}/escalations
```

### 13.4 Robot API

```txt
GET    /api/v1/robots
POST   /api/v1/robots
GET    /api/v1/robots/{id}
PUT    /api/v1/robots/{id}
DELETE /api/v1/robots/{id}
GET    /api/v1/robots/{id}/runs
POST   /api/v1/robots/{id}/runs
```

### 13.5 Asset API

```txt
GET    /api/v1/assets
POST   /api/v1/assets
GET    /api/v1/assets/{id}
PUT    /api/v1/assets/{id}
DELETE /api/v1/assets/{id}
GET    /api/v1/assets/expiring-soon
```

### 13.6 Report API

```txt
GET /api/v1/reports/daily
GET /api/v1/reports/monthly
GET /api/v1/reports/yearly
GET /api/v1/reports/tickets
GET /api/v1/reports/robots
GET /api/v1/reports/assets
GET /api/v1/reports/export/csv
```

### 13.7 Knowledge Base API

```txt
GET    /api/v1/knowledge-base
POST   /api/v1/knowledge-base
GET    /api/v1/knowledge-base/{id}
PUT    /api/v1/knowledge-base/{id}
DELETE /api/v1/knowledge-base/{id}
```

---

## 14. Standard API Response

### Success Response

```json
{
  "success": true,
  "message": "Success",
  "data": {}
}
```

### Error Response

```json
{
  "success": false,
  "message": "Validation error",
  "errors": [
    {
      "field": "title",
      "message": "title is required"
    }
  ]
}
```

### Paginated Response

```json
{
  "success": true,
  "message": "Success",
  "data": [],
  "pagination": {
    "page": 1,
    "limit": 10,
    "total_data": 100,
    "total_page": 10
  }
}
```

---

## 15. Business Rules

### Ticket Number Rule

Ticket number format:

```txt
OPS-{YEAR}-{RUNNING_NUMBER}
```

Example:

```txt
OPS-2026-000001
```

### SLA Rule MVP

Default SLA by priority:

| Priority | SLA Target |
|---|---|
| LOW | 5 business days |
| MEDIUM | 3 business days |
| HIGH | 1 business day |
| CRITICAL | 4 hours |

### SLA Breach Rule

Ticket dianggap breach jika:

1. Current time > `sla_due_at`.
2. Status belum `RESOLVED`, `CLOSED`, atau `CANCELLED`.

### Auto Activity Rule

System harus membuat ticket activity otomatis ketika:

1. Ticket dibuat.
2. Status berubah.
3. Assignee berubah.
4. Priority berubah.
5. Comment ditambahkan.
6. Escalation dibuat.
7. Root cause diupdate.
8. Resolution diupdate.

### Robot Success Rate Rule

```txt
success_rate = success_run / total_run * 100
```

Untuk MVP, success rate dihitung dari robot run logs berdasarkan date range.

### License Expiring Soon Rule

Asset dianggap expiring soon jika:

```txt
expired_date <= today + reminder_days
```

Default reminder days: 30.

---

## 16. Seed Data MVP

Untuk memudahkan demo dan vibe coding, siapkan seed data:

### Users

1. Admin User
2. RPA Support User
3. Business User
4. Manager User

### Departments

1. RPA Team
2. Finance
3. Operations
4. IT Infrastructure
5. Network

### Applications

1. Office 365
2. SAP
3. Internal Web Portal
4. Core Banking
5. Email Gateway

### Robots

1. POD Payment Regress
2. Daily Settlement Bot
3. Invoice Download Bot
4. Report Generator Bot
5. Reconciliation Bot

### Example Tickets

1. Office 365 login failed due to firewall whitelist.
2. Selector not found after application UI changed.
3. User requested enhancement for monthly report automation.
4. Robot failed because input file was missing.
5. License expiring soon for Office robot account.

---

## 17. Acceptance Criteria MVP

MVP dianggap selesai jika:

1. User dapat login dan logout.
2. User dapat membuka dashboard setelah login.
3. Dashboard menampilkan summary ticket, robot, dan license.
4. User dapat membuat ticket baru.
5. User dapat melihat list ticket dengan filter basic.
6. User dapat melihat detail ticket.
7. User dapat update status ticket.
8. User dapat assign ticket ke PIC.
9. User dapat menambahkan comment pada ticket.
10. Ticket activity otomatis tercatat.
11. User dapat membuat master robot.
12. User dapat menambahkan robot run log.
13. User dapat melihat chart robot success vs failed.
14. User dapat membuat asset/license.
15. User dapat melihat asset yang expiring soon.
16. User dapat melihat report basic berdasarkan date range.
17. User dapat export report ke CSV.
18. App dapat dijalankan lokal dengan dokumentasi setup.

---

## 18. Out of Scope for First Build

Jangan implement dulu kecuali core MVP sudah selesai:

1. Websocket real-time robot status.
2. Direct UiPath Orchestrator integration.
3. Direct Teams/WhatsApp notification.
4. Complex approval flow.
5. AI recommendation.
6. Advanced audit log.
7. Multi-company tenant.
8. PDF report advanced template.
9. Mobile responsive perfection.
10. SSO.

---

## 19. Recommended Development Phases

### Phase 1 — Foundation

1. Setup monorepo or separate backend/frontend folders.
2. Setup Go backend.
3. Setup Next.js frontend.
4. Setup PostgreSQL and Docker Compose.
5. Setup migration.
6. Setup base layout dashboard.
7. Setup auth.

### Phase 2 — Ticketing Core

1. Ticket CRUD.
2. Ticket list with filter.
3. Ticket detail.
4. Ticket status update.
5. Ticket assignment.
6. Ticket comments.
7. Ticket activities.

### Phase 3 — Dashboard & Reporting

1. Dashboard summary API.
2. Ticket charts.
3. Report page.
4. CSV export.

### Phase 4 — Robot Monitoring

1. Robot CRUD.
2. Robot run log CRUD.
3. Robot detail page.
4. Robot chart.
5. Link robot run to ticket.

### Phase 5 — Asset & License Monitoring

1. Asset CRUD.
2. Expiring soon logic.
3. License summary widget.
4. Asset report.

### Phase 6 — Knowledge Base & Polish

1. Knowledge base CRUD.
2. RCA search.
3. UI polish.
4. Empty/loading/error state.
5. Documentation.

---

## 20. Suggested Monorepo Structure

```txt
opspilot/
  backend/
  frontend/
  docs/
    PRD.md
    API.md
    DATABASE.md
  docker-compose.yml
  README.md
```

---

## 21. Environment Variables

### Backend `.env`

```env
APP_NAME=OpsPilot
APP_ENV=development
APP_PORT=8080
DATABASE_URL=postgres://opspilot:opspilot@localhost:5432/opspilot?sslmode=disable
JWT_SECRET=change-me
JWT_EXPIRES_IN=24h
LOG_LEVEL=debug
```

### Frontend `.env.local`

```env
NEXT_PUBLIC_APP_NAME=OpsPilot
NEXT_PUBLIC_API_BASE_URL=http://localhost:8080/api/v1
```

---

## 22. Docker Compose Requirements

Minimal services:

1. PostgreSQL.
2. Backend API.
3. Frontend app.

Optional for development:

1. pgAdmin.
2. Redis.

---

## 23. Definition of Done

Setiap fitur dianggap selesai jika:

1. Backend endpoint tersedia.
2. Frontend page/component tersedia.
3. Form validation tersedia.
4. Error handling tersedia.
5. Loading state tersedia.
6. Empty state tersedia jika berupa list/table.
7. Data tersimpan di database.
8. API response mengikuti standard response.
9. Minimal manual test berhasil.
10. Tidak ada hardcoded mock untuk fitur yang sudah terhubung API, kecuali seed data.

---

## 24. Agent Guardrails for Vibe Coding

Instruksi ini penting untuk Codex / AI agent agar tidak melebar.

### Must Follow

1. Fokus pada MVP terlebih dahulu.
2. Jangan membuat fitur advanced sebelum fitur core selesai.
3. Gunakan struktur modular.
4. Selalu update README jika ada command setup baru.
5. Selalu gunakan migration untuk perubahan database.
6. Selalu buat API response konsisten.
7. Selalu gunakan environment variable untuk config.
8. Jangan menyimpan password plain text.
9. Jangan membuat dependency terlalu banyak tanpa alasan kuat.
10. Jangan menghapus fitur existing tanpa instruksi.

### Do Not Build Yet

1. UiPath direct integration.
2. WhatsApp integration.
3. Teams integration.
4. AI root cause detection.
5. Complex approval workflow.
6. SSO.
7. Kubernetes deployment.

### Preferred Build Order

1. Project setup.
2. Database migration.
3. Auth.
4. Dashboard layout.
5. Ticket CRUD.
6. Ticket detail + comments + activities.
7. Dashboard summary.
8. Robot monitoring.
9. Asset monitoring.
10. Reports.
11. Knowledge base.

---

## 25. Initial Prompt for Codex

Use this prompt to start the build:

```txt
You are building OpsPilot, an RPA Operations, Ticketing, Monitoring, and Reporting Dashboard.

Read docs/PRD.md carefully and follow the MVP scope only.

Tech stack:
- Backend: Go
- Frontend: Next.js / React.js
- Database: PostgreSQL
- Local development: Docker Compose

Build order:
1. Setup project structure with backend, frontend, docs, and docker-compose.
2. Implement backend foundation with config, database connection, health check, standard response, and migration setup.
3. Implement authentication with JWT and hashed password.
4. Implement frontend login page and dashboard layout.
5. Implement ticket management MVP.

Important constraints:
- Do not build advanced features before MVP is complete.
- Keep code modular and clean.
- Use migrations for database changes.
- Use environment variables for config.
- Update README with setup steps.
- After each feature, summarize completed work and remaining tasks.
```

---

## 26. Future Enhancement Ideas

1. Integration with UiPath Orchestrator API.
2. Webhook endpoint for robot status update.
3. Email notification.
4. Microsoft Teams notification.
5. Daily report auto-send.
6. PDF report export.
7. AI-assisted root cause suggestion.
8. SLA calendar excluding holidays.
9. Advanced permission per department.
10. Public requester portal.
11. Audit log advanced.
12. Real-time robot status.
13. Dark mode.
14. Kanban ticket board.
15. Calendar agenda for robot schedule and license renewal.

---

## 27. Success Metrics

OpsPilot dianggap berhasil jika:

1. Semua issue RPA tercatat dalam ticket.
2. Status pekerjaan lebih mudah dimonitor.
3. Eskalasi terdokumentasi jelas.
4. Report harian/bulanan bisa dibuat lebih cepat.
5. Robot failed lebih cepat diketahui dan ditindaklanjuti.
6. License/account expired bisa dicegah sebelum berdampak ke operasional.
7. Knowledge base membantu mengurangi analisis berulang.

---

## 28. Final Notes

OpsPilot harus dibangun sebagai produk internal yang sederhana tetapi scalable. Prioritas utama bukan membuat fitur sebanyak mungkin, melainkan membuat alur kerja operasional RPA menjadi lebih rapi, terukur, dan mudah dilaporkan.

Fokus pertama adalah:

```txt
Login → Dashboard → Ticketing → Robot Monitoring → Reporting
```

Setelah MVP stabil, fitur license, escalation, notification, dan knowledge base bisa diperkuat secara bertahap.
