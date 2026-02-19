# Lambda API V4

## Overview
This repository contains the source code for the **API-V4** Lambda function that runs in the *core* AWS account. The service is built with [FastAPI](https://fastapi.tiangolo.com/) and packaged for deployment as an AWS Lambda function behind Amazon API Gateway. All application code that is uploaded to the Lambda function lives in the `API-V4/` directory.

## Runtime
- **Python**: 3.10
- **Framework**: FastAPI
- **ORM**: SQLAlchemy with Pydantic models for data validation

## Repository layout
```
├── API-V4/                 # Lambda handler, application code, and dependencies
│   ├── core/               # Configuration helpers and global settings
│   ├── models/             # SQLAlchemy models for domain entities
│   ├── repository/         # Data-access helpers (SessionDAL, CRUD helpers, etc.)
│   ├── routers/            # FastAPI routers that expose the HTTP API surface
│   ├── schemas/            # Pydantic schemas shared by the routers
│   ├── services/           # Business logic modules used by the routers
│   └── tests/              # Pytest suites that exercise selected endpoints
├── database/               # Database migrations and SQL helpers used by the API
├── docs/                   # User-facing API documentation
├── CLOUDWATCH_QUERIES.md   # Saved CloudWatch Insights queries for observability
└── README.md               # Repository overview (this file)
```

## Environment configuration
The Lambda function reads a single environment variable called `MYSQL` that contains the MySQL connection details in the following format:

```
{user}:{password}:{host}:{port}:{database_name}
```

The application automatically parses this value and builds the SQLAlchemy DSN at runtime, so no `.env` files are required.

## Required headers
All HTTP requests to the Lambda API must include the following headers:

- `Authorization`: Token retrieved from the account configuration panel in [my.clickie.io](https://my.clickie.io/).
- `Account`: Numeric or UUID identifier of the active account, also available from the same configuration panel.

Both headers are defined as collection variables in `postman/lambda-api-v4.postman_collection.json` so they can be reused across all requests. Populate them before running the collection locally or through the Postman desktop client.

## Documentation workflow
When adding, removing, or modifying endpoints you must keep the written and executable documentation synchronized. Update the Markdown guides under `docs/`, refresh the Postman collection stored at `postman/lambda-api-v4.postman_collection.json`, and revise `roadmap.md` so its coverage table matches the codebase. Capture these adjustments in the same pull request as the API change to avoid drift between artifacts.

## Local development
1. Create and activate a Python 3.10 virtual environment.
2. Install dependencies with `pip install -r API-V4/requirements.txt`.
3. Run the FastAPI application locally with your preferred ASGI server (for example `uvicorn main:app --reload`) and export the `MYSQL` variable as described above.
4. Execute the automated tests with `pytest` from the repository root.

## Deployment
The contents of `API-V4/` are zipped and uploaded directly to the AWS Lambda function. Ensure that the dependency versions in `requirements.txt` remain compatible with Python 3.10 and the AWS Lambda execution environment.

## Release changelog

### Version 4.3.2 (2026-01-25)
- **dashboard_widgets**: Expanded dashboard widget configuration handling to support nested form_data trees with main forms, subforms, and id_form_data-aware updates and deletes; defaults now seed required inputs and at least one subform entry; creation payloads validate required inputs and select-option values; subforms now store the parent form_data identifier as their `id_resource`; form_data parsing handles double-encoded JSON; config_data rejects unknown keys.
- **docs**: Updated dashboard widget documentation, refreshed the Postman collection examples, and aligned the roadmap coverage notes.

### Version 4.3.1 (2026-01-21)
- **monitoring**: Added `trigger_parameters` support for monitor triggers, persisting payloads in form data tied to the trigger type form; added `GET /monitor_templates` to list monitor-ready templates.
- **docs**: Updated monitor trigger and template documentation, refreshed the Postman collection, aligned the roadmap coverage, and synchronized trigger sample payloads.

### Version 4.3.0 (2026-01-15)
- **accounts**: Clearance 1 sessions can set or update `id_environment` when creating or updating accounts; other clearances remain scoped to the session environment. Account responses now include environment metadata.
- **environments**: Added `GET /environments` to list available environments for the authenticated session. (Release notes in progress; additional items may be merged before the release is finalized.)
- **search**: `/search/metrics` now returns `400` errors with validation details when filters are invalid instead of surfacing unhandled `500` exceptions.
- **search**: `/search/metrics` now treats missing or empty `resource_filter` payloads as metadata-only calls, skipping relationship joins and DISTINCT clauses unless resource scoping is explicitly provided.
- **search**: `/search/metrics` now uses a dedicated query path for metadata-only requests, while resource-scoped searches join `resource_relationships`, preventing errors when `resource_filter` is omitted.
- **search**: Moved `/search/metrics` relationship filtering into the resource-scoped query helper, keeping the main dispatcher focused on validation and routing.
- **search**: `/search/metrics` now builds parameter-only queries separately before applying resource relationship filters.
- **search**: `/search/metrics` now drops unknown `return_fields` and defaults to the full field set when none are valid, while only `id_resource` is disallowed in parameter filters.
- **search**: `/search/metrics` now avoids selecting a literal `id_resource` for metadata-only requests and only appends `id_resource` when resource filters are active.
- **search**: `/search/metrics` now always returns `id_resource` when a `resource_filter` is provided, regardless of `return_fields`.
- **search**: Added `POST /search/metrics` under the `search` tag to filter metrics by linked assets or metric metadata with customizable return fields and pagination support. Relationship traversal uses `resource_type: asset` plus a `direction` flag that defaults to `child` and surfaces `id_resource` values that mirror the requested `resource_ids`, while metadata-only searches skip `resource_relationships` entirely. Resource filters accept only `in` or `not_in` operators and parameter operators are validated before building queries. Metrics entity lookups are bypassed during relationship filtering by pinning the entity identifier to `7` for metrics.
- **docs**: Documented the metrics search endpoint in `docs/Search/Metrics.md`, refreshed the Postman collection, and updated the roadmap coverage to reflect the new search surface.

### Version 4.2.0 (2025-12-15)
- **forms / form_inputs / form_input_types**: Added read endpoints for form catalogs, including input definitions and renderer types with clearance 7 for reads.
- **dashboards**: Parent dashboard relationships are supported; archived filtering is optional for clearances 1–2 and defaults to non-archived for others; creation/deletion use clearance 2, updates clearance 5, reads clearance 8.
- **dashboard_widgets**: CRUD interacts with dashboard widget instances, storing configuration in `form_data`; list views omit `config_data` while detail responses return it; updates merge partial `config_data` payloads; creation/deletion clearance 6, read/update clearance 7.
- **widgets / widget_types**: Standalone widget catalog supports create/delete (clearance 1), update (clearance 2), read (clearance 7); widget types remain read-only (clearance 7).
- **accounts**: Clearance 1 can list or fetch any account without being scoped to memberships or the `Account` header; higher clearances remain scoped and keep archived-filtering safeguards tied to their roles.
- **collaborators**: Clearance 1 can manage collaborators across accounts; collaborator APIs return the collaborator payload after mutations and keep timestamps consistent with the database schema.
- **users**: User-account associations honor database-managed `created_at` defaults so server timestamps are preserved.
