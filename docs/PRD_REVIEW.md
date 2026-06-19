# PRD Review — OpsPilot

## Summary

PRD OpsPilot sudah kuat untuk menjadi sumber requirement awal. Isinya sudah mencakup problem, target pengguna, MVP scope, modul utama, API awal, database direction, seed data, acceptance criteria, dan guardrails.

Namun untuk workflow AI coding agent, PRD saat ini masih terlalu banyak memuat campuran antara:

1. Product requirement.
2. Architecture direction.
3. API requirements.
4. Database direction.
5. Development phases.
6. Agent guardrails.
7. Initial Codex prompt.

Itu tidak salah, tetapi jika semua selalu dimasukkan ke konteks agent, token akan boros dan agent bisa mudah melebar.

## Recommended Adjustment

Jangan buang isi PRD. Pisahkan menjadi dokumen kerja berikut:

| Existing PRD Content | Moved / Repeated Into |
|---|---|
| Ringkasan produk, user, goals, non-goals, MVP scope | `docs/PRD.md` |
| Backend/frontend/database direction | `docs/ARCHITECTURE.md` |
| Endpoint list, response format, DTO | `docs/API_CONTRACT.md` |
| Database entities, relation, migration notes | `docs/DB_SCHEMA.md` or migrations |
| Build order dan development phases | `backlog/BACKLOG.md` |
| Agent guardrails | `AGENTS.md` |
| Current progress | `backlog/STATUS.md` |
| One feature execution detail | `tasks/TASK-xxx.md` |
| Technical choices | `docs/DECISIONS.md` |

## Scope Clarification for MVP

MVP should focus on this flow first:

```txt
Login → Dashboard Layout → Ticket CRUD → Ticket Detail → Comments/Activities → Robot Run Logs → Asset Expiring Soon → Basic Reports
```

The following should stay out of the first build:

- Direct UiPath Orchestrator integration.
- Teams/WhatsApp notification.
- Websocket real-time robot status.
- AI root cause recommendation.
- SSO.
- Multi-tenant company setup.
- Advanced approval workflow.
- Advanced PDF/Excel export.

## Risk in Current PRD

| Risk | Why It Matters | Mitigation |
|---|---|---|
| Scope is broad | Agent may build too many modules at once | Use task files and execute one task per session |
| API list exists but not detailed | FE/BE contract can drift | Use `docs/API_CONTRACT.md` |
| Architecture has options | Agent may choose random libraries | Lock choices in `docs/DECISIONS.md` |
| Build order exists but not ticketized | Agent may skip foundation | Use `backlog/BACKLOG.md` and `/tasks` |
| Agent guardrails inside PRD | Agent may not always read them | Move into `AGENTS.md` |

## Recommended Next Step

Do not ask Codex to generate and implement everything in one prompt.

Use this sequence:

1. Ask Codex to copy/confirm docs and structure only.
2. Ask Codex to review `docs/ARCHITECTURE.md` and `docs/API_CONTRACT.md`.
3. Ask Codex to execute `tasks/TASK-001-project-foundation.md`.
4. Review diff.
5. Continue one task at a time.
