# Codex Prompts — OpsPilot

## 1. Initial Repo Setup

```txt
Read AGENTS.md, docs/PRD.md, docs/ARCHITECTURE.md, docs/API_CONTRACT.md, docs/DECISIONS.md, backlog/BACKLOG.md, and backlog/STATUS.md.

Execute only:
tasks/TASK-001-project-foundation.md

Before coding:
- Summarize the task scope.
- Provide a short implementation plan.
- List files that will be created or changed.

Rules:
- Do not implement business features yet.
- Do not implement auth, ticketing, dashboard data, robot monitoring, or reporting yet.
- Only prepare the initial repo foundation.
- Follow AGENTS.md.
- Keep changes small and reviewable.

After coding:
- Run relevant checks if available.
- Update tasks/TASK-001-project-foundation.md status.
- Update backlog/STATUS.md.
- Summarize completed work, changed files, and next recommended task.
```

## 2. Execute One Task

```txt
Read AGENTS.md and execute only this task:

tasks/TASK-XXX-name.md

Rules:
- Do not implement out-of-scope items.
- Do not change architecture decisions unless necessary.
- If a decision changes, update docs/DECISIONS.md.
- Run relevant checks.
- Update the task status and backlog/STATUS.md.
```

## 3. Review Latest Diff

```txt
Review the latest diff against the active task acceptance criteria.

Check:
- correctness,
- scope creep,
- missing validation,
- API contract mismatch,
- UI loading/error/empty state,
- missing README/status updates.

Do not implement changes yet.
Return findings grouped by Critical, Major, Minor, and Nice to have.
```

## 4. Generate More Task Files

```txt
Read docs/PRD.md, docs/ARCHITECTURE.md, docs/API_CONTRACT.md, backlog/BACKLOG.md, and existing tasks.

Generate missing task files for the next epic only.

Each task must include:
- status,
- priority,
- goal,
- scope,
- out of scope,
- acceptance criteria,
- relevant docs,
- agent instructions.

Do not implement code.
```

## 5. Antigravity UI Validation

```txt
Run the app locally if possible and validate the current frontend task.

Check:
- page loads,
- layout consistency,
- sidebar navigation,
- form states,
- table states,
- chart visibility,
- responsive desktop/tablet behavior.

Capture findings and update the active task with QA notes.
Do not implement unrelated fixes.
```
