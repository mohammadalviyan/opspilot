# Status — OpsPilot

## Current Phase

Foundation

## Active Task

None

## Last Completed Task

`tasks/TASK-001-project-foundation.md`

## Next Recommended Task

`tasks/TASK-002-docker-compose-env.md`

## Progress Summary

- PRD exists.
- AI-ready documentation pack prepared.
- Architecture, API contract, decisions, backlog, task files, and skills are prepared as starting point.
- Initial monorepo foundation has been created with `backend/` and `frontend/` folders.
- Root `.gitignore`, environment examples, and README project structure notes are in place.

## Open Questions

- Confirm whether backend router will be Gin or Chi. Current proposal: Gin.
- Confirm whether auth token will be stored via httpOnly cookie or temporary local storage for MVP.
- Confirm whether Next.js should use App Router or Pages Router. Current proposal: App Router.

## Blockers

None.

## Recent Decisions

- Use one task per agent session.
- Keep future integrations out of MVP.
- Use monorepo with backend and frontend folders.
