# TASK-010 — Ticket Create and List API

## Status

todo

## Priority

P0

## Goal

Implement ticket create and list API endpoints.

## Scope

- Implement `POST /api/v1/tickets`.
- Implement `GET /api/v1/tickets`.
- Support pagination and basic filters.
- Generate ticket number.
- Calculate default SLA due date based on priority.

## Out of Scope

- Do not implement comments.
- Do not implement frontend.
- Do not implement attachments.

## Acceptance Criteria

- Ticket can be created.
- Ticket list supports pagination.
- Basic filters work.
- Standard response is used.
- Create ticket activity is recorded.

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
