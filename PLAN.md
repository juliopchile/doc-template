# Active Work Plan

`PLAN.md` tracks the current active work only. It should contain enough context for a new developer or agent to continue the current task without reading chat history.

Do not use this file as a changelog, roadmap, or architecture document. Completed work should be condensed into `PROJECT_ROADMAP.md`, and stable design information should live in `PROJECT_STRUCTURE.md`.

## Current Status

**Current phase:** Phase 2 — Inventory CRUD

**Status:** In progress

**Immediate next step:** Implement stock adjustment validation in `src/services/inventory_service.py`.

## Active Task: Stock Adjustment Validation

### Objective

Ensure stock cannot be adjusted below zero and that every adjustment records an audit event.

### Files In Scope

- `src/services/inventory_service.py`
- `src/models/item.py`
- `src/models/audit_event.py`
- `tests/test_inventory_adjustments.py`

### Tasks

- Add validation for negative stock results.
- Create audit events for each successful adjustment.
- Return a consistent error response when validation fails.
- Add unit tests for successful adjustment, negative adjustment failure, and audit creation.

### Constraints

- Keep route handlers thin; validation belongs in the service layer.
- Use existing response format: `{ "data": ..., "error": ... }`.
- Do not change database schema in this task.

### Verification

Run focused tests:

```sh
make test TEST=tests/test_inventory_adjustments.py
```

Then run the full suite:

```sh
make test
```

### Blockers

None.

## After Completion

When this work is complete:

- Replace this detailed section with the next active task.
- Add a short completed-work summary to `PROJECT_ROADMAP.md`.
