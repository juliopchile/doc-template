# Documentation Template Project

> AI agents: read `AGENTS.md` before working in this template project.

This is a standalone fake project that demonstrates a reusable documentation system for software repositories. It is intended to be copied into new projects and adapted by replacing placeholders such as `{project_name}`, `{framework}`, and `{main_command}`.

The template separates permanent documentation from active work tracking:

| File | Purpose |
|---|---|
| `README.md` | Human entry point: overview, setup, usage |
| `PROJECT_STRUCTURE.md` | Architecture, repository layout, components, endpoints/messages |
| `PROJECT_ROADMAP.md` | Project timeline, completed phases, pending work, technical debt |
| `PLAN.md` | Active work only: current task, blockers, immediate next steps |
| `AGENTS.md` | Agent-facing instructions, conventions, and reading order |
| `SKILL.md` | Generic cross-agent skill-writing guide |
| `skills/` | Bundled cross-platform agent skills (`skill-creator`, `orchestrating-agents`), mirrored into `.claude/`, `.opencode/`, `.codex/`, and `.agents/` |

## Setup

Copy the template into a new project, then update placeholders:

```sh
cp -R doc-template {project_name}
cd {project_name}
```

Then edit:

- `{project_name}`: real project name
- `{framework}`: framework or runtime, such as FastAPI, Next.js, Django, Rails
- `{main_command}`: primary run command, such as `make dev` or `npm run dev`
- Fake examples: replace with real architecture, commands, and endpoints

## Usage

Use this template when starting a repository that should be easy for humans and AI agents to understand quickly.

Recommended order for maintainers:

1. Fill out `README.md` with a concise overview and setup instructions.
2. Fill out `PROJECT_STRUCTURE.md` with the actual architecture and file layout.
3. Fill out `PROJECT_ROADMAP.md` with initial phases and known limitations.
4. Create or update `PLAN.md` only when active implementation work exists.
5. Keep `AGENTS.md` compact and repo-specific.

## Working With Agents

This documentation system is designed so AI agents can ramp up from an empty context: `AGENTS.md`/`README.md` tell them **where** information lives, `PLAN.md` holds the active work, and completed work is condensed into `PROJECT_ROADMAP.md`. On top of it, the `orchestrating-agents` skill defines a staged delegation workflow: an orchestrator agent coordinates fresh sub-agents through three prompts per stage (familiarization → stage summary → authorized execution), avoiding context rot and keeping token usage focused. See `skills/orchestrating-agents/` and the roles section in `AGENTS.md`.

## Example: Fictional Sample Project

Example project: **Inventory Tracker API**

Inventory Tracker is a fictional REST API for managing warehouse stock levels.

- Framework: FastAPI
- Database: PostgreSQL
- Main command: `make dev`
- Main endpoints: `GET /items`, `POST /items`, `PATCH /items/{id}/stock`

In that project:

- `README.md` explains what the API does and how to run it.
- `PROJECT_STRUCTURE.md` documents routers, services, schemas, database models, and endpoints.
- `PROJECT_ROADMAP.md` tracks phases such as Foundation, Inventory CRUD, Auth, Reporting.
- `PLAN.md` tracks the current active task, such as implementing stock adjustment validation.
- `AGENTS.md` tells AI agents which commands to use and which project conventions are easy to miss.

## Documentation Rule

Do not put everything in one file. Put stable project knowledge in permanent docs and current implementation details in `PLAN.md`.
