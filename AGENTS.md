# AGENTS.md — OpsPilot

## Working Mode

- Always work from exactly one active task file in `/tasks`.
- Do not implement features outside the active task scope.
- Before coding, summarize the task goal, affected files, and validation plan.
- Keep changes small, reviewable, and aligned with the MVP.
- After coding, run relevant checks and update:
  - the active task status,
  - `backlog/STATUS.md`,
  - `docs/DECISIONS.md` if a technical decision was made.

## Source of Truth

Read these files when relevant:

- Product requirement: `docs/PRD.md`
- Architecture: `docs/ARCHITECTURE.md`
- API contract: `docs/API_CONTRACT.md`
- Decisions: `docs/DECISIONS.md`
- Backlog: `backlog/BACKLOG.md`
- Current status: `backlog/STATUS.md`

## Build Order

Follow this order unless the active task says otherwise:

1. Foundation
2. Database migration
3. Auth
4. Dashboard layout
5. Ticketing core
6. Ticket detail, comments, and activities
7. Dashboard summary and charts
8. Robot monitoring
9. Asset monitoring
10. Reports
11. Knowledge base

## Backend Rules

- Backend uses Go.
- Follow handler → service → repository layering.
- Handler only handles HTTP request/response mapping.
- Service contains business logic.
- Repository contains database access only.
- Use migrations for database schema changes.
- Use environment variables for configuration.
- Use consistent API response shape.
- Do not store plain text passwords.
- Do not add new dependencies without explaining the reason.

## Frontend Rules

- Frontend uses Next.js / React.
- Use reusable components for layout, forms, tables, badges, and charts.
- All API calls must go through a shared API client.
- Every list page must have loading, error, empty state, and pagination where relevant.
- Do not hardcode production data. Use seed/mock data only where explicitly allowed for MVP.

## Definition of Done

A task is done only when:

- Acceptance criteria in the task are satisfied.
- Relevant build/lint/test/manual checks are executed or explained if skipped.
- Existing behavior is not broken.
- The task file status is updated.
- `backlog/STATUS.md` is updated.
- New setup commands are documented in `README.md`.

## Guardrails

- Do not build advanced/future features before MVP is stable.
- Do not implement UiPath, Teams, WhatsApp, SSO, AI RCA, or websocket real-time monitoring unless the active task explicitly asks.
- Do not silently rewrite architecture decisions. If a decision must change, update `docs/DECISIONS.md`.
- Do not delete existing files or features unless explicitly asked.
