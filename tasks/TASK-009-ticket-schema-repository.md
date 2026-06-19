# TASK-009 — Ticket Schema and Repository

## Status

todo

## Priority

P0

## Goal

Create core ticketing database schema and repository layer.

## Scope

- Add migrations for tickets, ticket_comments, ticket_activities, escalations.
- Add ticket models.
- Add repository methods for create, list, detail, update, status, assign.
- Add pagination query support.

## Out of Scope

- Do not implement frontend ticket pages.
- Do not implement advanced attachment upload.

## Acceptance Criteria

- Migrations run successfully.
- Repository compiles.
- Ticket number generation strategy is documented or implemented.
- Activity creation support exists.

## Relevant Docs

- `AGENTS.md`
- `docs/PRD.md`
- `docs/ARCHITECTURE.md`
- `docs/API_CONTRACT.md`
- `docs/DECISIONS.md`
- `backlog/STATUS.md`

## Agent Instructions

- Work only on this task.
- Before coding, write a short implementation plan.
- Do not implement out-of-scope items.
- After coding, run relevant checks.
- Update this task status.
- Update `backlog/STATUS.md`.
