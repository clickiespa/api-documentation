# Changelog

Release notes are organized by FastAPI tag for each API version exposed in `/docs`.

## Version 4.3.2 (2026-01-25)

### dashboard_widgets
- Expanded dashboard widget configuration handling to support nested form_data trees with main forms, subforms, and id_form_data-aware updates and deletes.
- Widget configuration defaults now seed required inputs and at least one subform entry when no payload is provided.
- Dashboard widget config validation now enforces required inputs and select-option constraints on creation payloads.
- Corrected subform persistence so nested entries store the parent form_data identifier as their `id_resource`.
- Hardened form_data parsing to handle double-encoded JSON so stored configuration values render correctly.
- Rejects config_data fields that are not defined in the form inputs to prevent storing unknown keys.

### docs
- Updated dashboard widget documentation, refreshed the Postman collection examples, and aligned the roadmap coverage notes.

## Version 4.3.1 (2026-01-21)

### monitoring
- Added `trigger_parameters` handling for monitor triggers, storing the payload in form data keyed to the trigger type form.
- Exposed `GET /monitor_templates` to list monitor-ready notification templates.

### docs
- Updated monitor trigger documentation and examples, added monitor template documentation, refreshed the Postman collection, updated the roadmap coverage, and aligned trigger sample payloads.

## Version 4.3.0 (2026-01-15)

### accounts
- Clearance 1 sessions can set or update `id_environment` when creating or updating accounts; other clearances remain scoped to the session environment.
- Account responses now include environment metadata alongside `id_environment`.

### environments
- Added `GET /environments` to list available environments for the authenticated session.

### search
- `/search/metrics` now translates invalid filter payloads into `400` responses instead of uncaught `500` errors, surfacing validation feedback when relationships or operators are misused.
- `/search/metrics` now treats missing or empty `resource_filter` payloads as metadata-only requests, skipping relationship joins and `DISTINCT` clauses unless resource scoping is explicitly provided.
- `/search/metrics` now routes metadata-only requests through a dedicated query path while resource-scoped searches join `resource_relationships`, avoiding errors when `resource_filter` is omitted.
- `/search/metrics` now encapsulates `resource_relationships` filtering inside the resource-scoped query path, leaving `get_metrics_with_filters` focused on shared validation and dispatch.
- `/search/metrics` now builds base parameter queries separately from resource-scoped queries, applying relationship joins only after the base query is constructed.
- `/search/metrics` now drops unknown `return_fields` and defaults to full field sets when none are valid, while only `id_resource` is blocked for parameter filtering.
- `/search/metrics` no longer selects a literal `id_resource` for metadata-only requests, and only appends `id_resource` when resource filters are active to avoid mixed-column selection errors.
- `/search/metrics` now always includes `id_resource` when a `resource_filter` is provided, regardless of `return_fields`, while metadata-only queries omit it entirely.

### search
- Added `POST /search/metrics` under the `search` tag to filter metrics by linked assets or metric metadata with customizable return fields and pagination. Relationship traversal applies only when `resource_type` is `asset`.
- Metrics search can now traverse `resource_relationships` in either direction using an explicit `direction` flag that resolves `resource_type` values from `eav_entities` and returns `id_resource` values that mirror the requested `resource_ids`, defaulting to the `child` direction when omitted.
- Resource filters enforce `in` or `not_in` operators while parameter filters validate against the full set of supported operators to prevent unsupported comparisons from reaching the query builder.
- The metrics entity identifier is pinned to `7` during relationship filtering to avoid redundant lookups when evaluating resource-scoped queries.

### docs
- Documented the new metrics search endpoint, updated the Postman collection, and refreshed the roadmap coverage for metrics filtering.

## Version 4.2.0 (2025-12-15)


This release consolidates the changes shipped in the latest `development` → `main` merge (starting with merge commit `a09eb62`). Use that merge's commit list as the source of truth for any follow-up release note or changelog adjustments. For 4.2.x documentation updates, reference the consolidated development → main merge pull request for the targeted release rather than individual feature PRs so tooling can programmatically collect the relevant commits from a single entry point.

### forms
- Exposes catalog endpoints to list forms and retrieve individual definitions (clearance 7 for read operations).
- Form records include environment/account scoping metadata plus descriptive names and optional descriptions.

### form_inputs
- Lists and fetches input definitions scoped to a form (read clearance 7).
- Input metadata covers labels, defaults, placeholders, select options, multiplicity flags, validation requirements, and attribute metadata used by custom form rendering.

### form_input_types
- Provides the catalog of input renderers (read clearance 7) such as text, select, textarea, resource pickers, nested forms, and other UI primitives.

### dashboards
- Supports creation and deletion with clearance 2, updates with clearance 5, and reads with clearance 8.
- Dashboards store optional parent relationships (`id_parent_dashboard`) and color metadata; archived filtering is optional for clearances 1–2 and defaults to non-archived for higher clearances.

### dashboard_widgets
- Manages dashboard widget instances: creation/deletion with clearance 6, read/update with clearance 7.
- Widget configuration is persisted in `form_data`; list responses omit `config_data` while detail responses include the stored configuration.
- Partial updates to `config_data` merge with existing values so unspecified keys remain intact.

### widgets
- Catalog endpoints for widgets: creation/deletion with clearance 1, updates with clearance 2, reads with clearance 7.
- Widgets link to widget types and optional forms used to seed dashboard widget configuration payloads.

### widget_types
- Read-only catalog of widget type definitions (clearance 7) used to classify widgets.

### accounts
- Clearance 1 sessions can list or fetch any account without being scoped to memberships or the `Account` header, while higher clearances remain scoped to their linked accounts.
- Account retrieval keeps archived-filtering safeguards for higher roles and aligns pagination with the shared repository patterns.

### collaborators
- Clearance 1 can manage collaborators across accounts (create, update, delete) without being limited to the current `Account` header context.
- Collaborator APIs now return the collaborator payload after create/update operations and consistently include timestamp fields that match the database schema.

### users
- User-account associations align `created_at` defaults with database-managed timestamps to avoid client-provided values overriding server defaults.
