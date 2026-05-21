# AGENTS.md

This file is written for AI coding agents working in `{project_name}`. It should help agents avoid avoidable mistakes and ramp up quickly.

## Primary Documentation Sources

Read these files before making changes:

1. `README.md` — human overview, setup, usage, and quick start
2. `PROJECT_STRUCTURE.md` — architecture, repository layout, endpoints/messages, conventions
3. `PROJECT_ROADMAP.md` — project progress, completed phases, pending work, technical debt
4. `PLAN.md` — active work only; consult when it exists and the user asks for implementation work

`PLAN.md` is not permanent project documentation. Use it to understand the current task, blockers, and next steps. Some projects may also include other temporary notes or investigation files; treat those as supporting context, not authoritative docs.

## Agent Rules

- Prefer project commands from `README.md`, `PROJECT_STRUCTURE.md`, or the root task runner.
- Do not invent commands when scripts or Make targets already exist.
- If docs conflict with executable config, trust executable config and update docs if requested.
- Do not commit, push, release, or deploy unless the user explicitly asks.
- Preserve user changes; do not revert unrelated work.

## Generic AGENTS.md Creation Prompt

Agents often encounter prompts like this when asked to create or improve an `AGENTS.md` file. Treat it as a common pattern to adapt, not a mandatory script:

```md
Create or update `AGENTS.md` for `{project_name}`.

The goal is a compact instruction file that helps future agent sessions avoid mistakes and ramp up quickly. Every line should answer: "Would an agent likely miss this without help?" If not, leave it out.

Read the highest-value sources first:
- `README*`, root manifests, workspace config, lockfiles
- build, test, lint, formatter, typecheck, and codegen config
- CI workflows and pre-commit or task runner config
- existing instruction files such as `AGENTS.md`, `CLAUDE.md`, `.cursor/rules/`, `.cursorrules`, `.github/copilot-instructions.md`
- repo-local agent config such as `opencode.json`, `.agents/`, `.claude/`, or `.codex/`

If architecture is still unclear after reading config and docs, inspect a small number of representative code files to identify entrypoints, package boundaries, and execution flow. Prefer files that explain wiring over random leaf files.

Prefer executable sources of truth over prose. If docs conflict with config or scripts, trust the executable source and only keep what you can verify.

Extract high-signal facts:
- exact developer commands, especially non-obvious ones
- focused verification commands for one package, test, or feature
- required command order, such as `format -> lint -> test`
- monorepo or package boundaries
- framework quirks, generated code, migrations, build artifacts, env loading, dev servers, deployment flow
- testing prerequisites, fixtures, snapshots, required services, flaky or expensive suites
- repo-specific conventions that differ from defaults
- important constraints from existing instruction files worth preserving

Ask questions only if the repo cannot answer something important, such as undocumented release expectations or missing setup prerequisites.

Writing rules:
- Keep sections short.
- Include only repo-specific guidance.
- Exclude generic software advice.
- Exclude speculative claims.
- Link to permanent docs such as `PROJECT_STRUCTURE.md`, `PROJECT_ROADMAP.md`, `STYLES.md`, `DECISIONS.md`, or `ARCHITECTURE.md` when they exist.
- If `PLAN.md` exists, explain that it tracks active work and should be checked before continuing implementation.
```

## Additional Documentation Examples

A strong `AGENTS.md` should point to extra docs when they exist:

| File | When to Reference |
|---|---|
| `STYLES.md` | Frontend visual design, CSS, typography, component style rules |
| `DECISIONS.md` | Architecture decision records or major tradeoffs |
| `ARCHITECTURE.md` | Deep system design beyond `PROJECT_STRUCTURE.md` |
| `SECURITY.md` | Security reporting, auth rules, data handling requirements |
| `CONTRIBUTING.md` | PR workflow, branch naming, review requirements |
| `DEPLOYMENT.md` | Release process and infrastructure operations |

## Placeholder Commands

Replace these with real commands for `{project_name}`:

| Command | Purpose |
|---|---|
| `{install_command}` | Install dependencies |
| `{dev_command}` | Start local development server |
| `{test_command}` | Run tests |
| `{lint_command}` | Run linting |
| `{build_command}` | Build production artifact |
