# TASK-006 — Backend Auth Login, JWT, and Password Hashing

## Status

todo

## Priority

P0

## Goal

Implement backend authentication API for MVP.

## Scope

- Implement login endpoint.
- Implement auth middleware.
- Implement `/auth/me`.
- Implement logout response.
- Use hashed password verification.
- Use JWT.

## Out of Scope

- Do not implement SSO.
- Do not implement refresh token unless needed.
- Do not implement advanced permission yet.

## Acceptance Criteria

- User can login with seed user.
- Invalid credentials return standard error.
- Protected route can read user identity.
- Password is never stored or returned as plain text.

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
