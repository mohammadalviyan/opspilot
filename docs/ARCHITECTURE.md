# Architecture — OpsPilot

## 1. Overview

OpsPilot is an internal RPA Operations, Ticketing, Monitoring, and Reporting Dashboard.

The system is built as a monorepo with separate backend and frontend applications.

```txt
opspilot/
  backend/
  frontend/
  docs/
  backlog/
  tasks/
  docker-compose.yml
  AGENTS.md
```

## 2. Architecture Goals

- Keep MVP simple and locally runnable.
- Use modular backend structure.
- Keep frontend feature-based and reusable.
- Make API contract stable for FE/BE collaboration.
- Avoid over-engineering before MVP is complete.
- Make the project friendly for AI coding agents by using clear docs and task files.

## 3. Tech Stack Decisions

| Area | Decision |
|---|---|
| Backend | Go |
| HTTP Router | Gin |
| Database | PostgreSQL |
| DB Driver | pgx |
| Migration | golang-migrate |
| Password Hashing | bcrypt |
| Auth | JWT-based auth |
| Logger | slog or zerolog |
| Frontend | Next.js + React + TypeScript |
| Styling | Tailwind CSS |
| Component Base | shadcn/ui |
| Forms | react-hook-form |
| Validation | zod |
| Table | TanStack Table |
| Chart | Recharts |
| Local Runtime | Docker Compose |

## 4. Backend Structure

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
        routes.go
      user/
      ticket/
      robot/
      asset/
      dashboard/
      report/
      knowledge/
    pkg/
      response/
      validator/
      logger/
      jwt/
      password/
      pagination/
  migrations/
  docs/
  go.mod
  go.sum
```

## 5. Backend Layering Rules

### Handler

Responsibilities:

- Parse request.
- Validate request DTO.
- Call service.
- Return standard response.

Must not:

- Contain SQL query.
- Contain business rules.
- Directly access database.

### Service

Responsibilities:

- Business logic.
- Status transition logic.
- SLA calculation.
- Activity generation.
- Permission checks.

Must not:

- Know HTTP details.
- Write raw response object.

### Repository

Responsibilities:

- Database query.
- Transaction execution.
- Data persistence.

Must not:

- Contain HTTP logic.
- Contain UI or presentation logic.

## 6. Frontend Structure

```txt
frontend/
  app/
    login/
    dashboard/
    tickets/
    robots/
    assets/
    reports/
    knowledge-base/
  components/
    layout/
    ui/
    forms/
    charts/
    tables/
    feedback/
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
    constants.ts
    utils.ts
  hooks/
  types/
  styles/
  public/
```

## 7. Frontend Rules

- Page files should compose feature components.
- Business UI logic should live in `features/*`.
- Shared UI components should live in `components/*`.
- API calls must go through `lib/api.ts`.
- All forms must use validation.
- All list pages must handle loading, error, empty, and success states.
- Use status badges consistently.

## 8. Authentication Flow

MVP auth uses email/password and JWT.

```txt
User submits login form
  ↓
POST /api/v1/auth/login
  ↓
Backend validates credentials
  ↓
Backend returns token and user profile
  ↓
Frontend stores auth state
  ↓
Protected routes require authenticated user
```

Recommended MVP storage:

- Use httpOnly cookie if implemented early.
- If cookie auth delays MVP, use local storage temporarily and document the trade-off in `docs/DECISIONS.md`.

## 9. Database Overview

Core tables:

- users
- roles
- departments
- applications
- ticket_categories
- tickets
- ticket_comments
- ticket_activities
- robots
- robot_run_logs
- assets
- escalations
- knowledge_base
- sla_rules

## 10. API Design Principles

- Prefix all endpoints with `/api/v1`.
- Use consistent success/error/pagination response.
- Use query params for filtering list endpoints.
- Use PATCH for partial status/assignment changes.
- Soft delete where appropriate.
- Backend should generate `ticket_no`.

## 11. Local Development

Minimum local services:

```txt
PostgreSQL
Backend API
Frontend App
```

Optional:

```txt
pgAdmin
Redis
```

## 12. Observability for MVP

For MVP, keep observability simple:

- Structured logs.
- Request logging middleware.
- Error response consistency.
- Health check endpoint.

Future:

- OpenTelemetry.
- Sentry.
- Loki/Grafana.

## 13. Security Notes

- Password must be hashed.
- JWT secret must come from environment variable.
- Never commit `.env`.
- Validate all input.
- Restrict requester access to own tickets where role rules are implemented.
- Internal comments must not be visible to requester.

## 14. MVP Build Sequence

1. Project foundation.
2. Backend health check and config.
3. Database migration.
4. Auth.
5. Frontend login and dashboard shell.
6. Ticketing core.
7. Ticket detail, comments, activities.
8. Dashboard summary and chart.
9. Robot monitoring.
10. Asset monitoring.
11. Reports.
12. Knowledge base.
