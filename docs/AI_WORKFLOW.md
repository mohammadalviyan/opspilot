# AI Workflow — OpsPilot

## Goal

Make OpsPilot easy to build with Codex and Antigravity without repeatedly pasting the full PRD or losing context.

## Document Roles

| File | Purpose |
|---|---|
| `docs/PRD.md` | Product source of truth |
| `docs/ARCHITECTURE.md` | Technical direction |
| `docs/API_CONTRACT.md` | FE-BE contract |
| `docs/DECISIONS.md` | Long-term technical memory |
| `backlog/BACKLOG.md` | Epic and ticket list |
| `backlog/STATUS.md` | Current project progress |
| `tasks/*.md` | One executable unit of work |
| `AGENTS.md` | Permanent agent rules |
| `.agents/skills/*/SKILL.md` | Reusable agent workflows |

## Recommended Agent Loop

```txt
1. Pick one task from /tasks.
2. Read AGENTS.md.
3. Read only relevant docs.
4. Create a short implementation plan.
5. Execute the task.
6. Run checks.
7. Update task file.
8. Update backlog/STATUS.md.
9. Add decision to docs/DECISIONS.md if needed.
```

## Token Saving Rules

- Never paste the full PRD into every prompt.
- Reference file paths instead.
- Use short task files as the main context.
- Keep `STATUS.md` concise.
- Store decisions in `DECISIONS.md`.
- Use skills only for repeatable workflows.

## Codex Usage Pattern

Use Codex for:

- Implementing one task.
- Refactoring a contained module.
- Writing tests.
- Updating backend/frontend code.
- Reviewing diffs against acceptance criteria.

Avoid using Codex for:

- Big vague prompts like "build the entire app".
- Implementing multiple phases at once.
- Architecture changes without updating decisions.

## Antigravity Usage Pattern

Use Antigravity for:

- Planning and task grouping.
- UI/browser validation.
- Walkthrough artifacts.
- Multi-step workflows.
- Reviewing frontend behavior visually.

## MCP Recommendation

MCP is optional for MVP.

Use MCP only when it solves a clear problem:

| MCP | Use When |
|---|---|
| GitHub MCP | You want issues/PRs to be synced with tickets |
| PostgreSQL MCP | You want agent to inspect local DB safely |
| Figma MCP | You have a design file |
| Browser tools | You want UI validation and screenshots |
| Filesystem tools | You want controlled file context |

Do not add MCP just because it sounds advanced. For the first setup, repository docs and task files are enough.
