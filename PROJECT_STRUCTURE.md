# Project Structure

`PROJECT_STRUCTURE.md` documents how a project is organized and how its pieces fit together. It should help a new human or agent understand the repository without guessing from filenames alone.

This file normally documents:

- Repository layout and ownership of major directories
- Architecture and execution flow
- Frameworks, dependencies, and runtime assumptions
- Main components, modules, packages, or services
- HTTP endpoints, message formats, commands, or public APIs
- Project-specific conventions that affect implementation

## Example Project: Inventory Tracker API

Fictional REST API for managing warehouse inventory.

### Architecture

```
Client → FastAPI Router → Service Layer → PostgreSQL
                         → Audit Logger
```

### Repository Layout

```
inventory-tracker/
├── src/
│   ├── main.py                 # FastAPI entrypoint
│   ├── config.py               # Environment-based settings
│   ├── database.py             # Database connection/session helpers
│   ├── routers/                # HTTP route handlers
│   ├── services/               # Business logic
│   ├── models/                 # Database table definitions
│   ├── schemas/                # Request/response schemas
│   └── utils/                  # Reusable helpers
├── tests/                      # Unit and integration tests
├── docker/                     # Dockerfile and compose files
├── scripts/                    # One-off operational scripts
├── README.md
├── PROJECT_STRUCTURE.md
├── PROJECT_ROADMAP.md
├── PLAN.md
└── AGENTS.md
```

### Components

| Component | Purpose |
|---|---|
| `routers/` | Route-level validation, permissions, and exception mapping |
| `services/` | Core business logic, database writes, audit events |
| `schemas/` | Pydantic request and response models |
| `models/` | SQLAlchemy table definitions |
| `utils/` | Small reusable helpers |

### Dependencies

| Layer | Tool |
|---|---|
| Framework | FastAPI |
| Server | Uvicorn |
| Database | PostgreSQL |
| DB access | SQLAlchemy Core |
| Tests | pytest |

### Endpoints

| Method | Path | Purpose | Auth |
|---|---|---|---|
| `GET` | `/health` | Health check | none |
| `GET` | `/items` | List items | read |
| `GET` | `/items/{id}` | Get item detail | read |
| `POST` | `/items` | Create item | write |
| `PATCH` | `/items/{id}/stock` | Adjust stock | write |

Standard response:

```json
{ "data": {}, "error": null }
```

Error response:

```json
{ "data": null, "error": { "code": "invalid_request", "message": "Stock cannot be negative" } }
```

### Implementation Conventions

- Keep routers thin; put business rules in services.
- Use consistent response envelopes.
- Add tests for service behavior before adding broad endpoint coverage.
- Use `make` targets instead of raw framework commands.

## Mini Example: Weather Dashboard

Fictional frontend app that displays weather data for saved cities.

```
weather-dashboard/
├── src/
│   ├── app/                    # Route definitions
│   ├── components/             # Reusable UI components
│   ├── features/weather/       # Weather-specific data fetching and UI
│   ├── lib/api.ts              # API client
│   └── styles/                 # Global styles and tokens
├── public/                     # Static assets
├── tests/                      # Component and integration tests
└── docs/                       # Additional design docs
```

Architecture notes for this frontend:

- Framework: Next.js
- Data fetching: server components for initial city weather, client components for refresh actions
- Styling: Tailwind CSS plus local component classes
- Main user flow: select city → fetch forecast → display hourly and weekly cards
- Additional docs: `STYLES.md` for design tokens, `DECISIONS.md` for architectural tradeoffs

## Maintenance Rule

Update this file when structure, architecture, public endpoints, supported messages, or major conventions change. Do not use it to track temporary active work; use `PLAN.md` for that.
