# Project Roadmap

`PROJECT_ROADMAP.md` summarizes project progress over time. It is similar to a lightweight changelog plus forward-looking roadmap.

Unlike `PLAN.md`, it should not contain detailed active implementation notes. Once work is complete, condense the details into a short summary here.

## Timeline

| Phase | Status | Summary |
|---|---|---|
| 1. Foundation | Complete | Created app skeleton, config loading, Docker setup, and health check |
| 2. Inventory CRUD | In progress | Building item listing, item creation, and stock adjustment workflows |
| 3. Authentication | Pending | Add API key auth for write actions and session auth for dashboard users |
| 4. Reporting | Pending | Add stock movement reports and CSV export |
| 5. Testing & Hardening | Pending | Add integration tests, rate limits, and production readiness checks |

## Completed Work

### Phase 1: Foundation

Completed the initial project skeleton for the fictional Inventory Tracker API.

Delivered:

- Application entrypoint and health check
- Configuration loading from environment variables
- Dockerfile and local development compose file
- Basic test harness

Details were removed from `PLAN.md` after completion because they are no longer active work.

## In Progress

### Phase 2: Inventory CRUD

Current focus:

- Item list endpoint
- Item detail endpoint
- Item creation endpoint
- Stock adjustment validation
- Audit event creation

See `PLAN.md` for active implementation details.

## Pending Work

- Authentication and permissions
- Reporting endpoints
- CSV export
- Pagination and filtering improvements
- API documentation examples

## Technical Debt

- Test fixtures use duplicated sample item data; extract shared builders later.
- Error codes are strings today; consider centralizing them once the API surface stabilizes.
- Stock adjustment audit records are written synchronously; revisit if write volume grows.

## Known Limitations

- No real background job queue yet.
- No user-facing dashboard yet.
- No multi-warehouse transfer workflow yet.

## Roadmap Maintenance Rule

When a task, refactor, or implementation is completed, add a short summary here and remove detailed implementation notes from `PLAN.md`.
