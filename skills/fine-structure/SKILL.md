---
name: fine-structure
description: How to build, update, deploy and operate apps on the Fine Structure platform through its MCP server. Use whenever the user wants to deploy to Fine Structure, create or update a Fine Structure app, connect a domain, manage app data or secrets, or asks what Fine Structure can do.
---

# Working with Fine Structure

Fine Structure (finestructure.ai) is a full-stack app platform: AI-generated React apps with hosting, a managed database (entities), auth, custom domains, secrets, integrations, email marketing, media generation and autonomous AI agents with WhatsApp and email channels. Unlike static hosting, a deploy target here includes the backend: data, auth and automation live on the platform.

All operations go through the `finestructure` MCP server (105 tools). Machine-readable reference: https://finestructure.ai/api/mcp/docs

## Mental model

- An **app** is a set of JSX pages (`pages/`), shared components (`components/`), and entity schemas, served by the platform runtime. There is no arbitrary server code; backend behavior comes from entities, route policies, integrations and platform functions.
- **Entities** are the database. Define schemas (`create_entity_schema`), then apps read/write records through the generated data layer. Query and seed from outside with `query_entity`, `seed_entity`, `create_entity_records`.
- **Publishing** makes an app live at its platform URL and any verified custom domain.
- **Credits** meter AI generation and media. Reading and file writes are effectively free; `create_app`, `update_app` and `generate_*` spend credits.

## Core workflows

**Create from scratch**: `create_app` (rich prompt) → poll `get_job_status` → `get_app_links`.

**Iterate on files**: `get_app_files` → `read_app_file` → `write_app_file` / `patch_app_file` → `validate_app` → `publish_app`. For multi-file changes prefer change sets (`create_change_set` → `add_file_change`* → `validate_change_set` → `apply_change_set`).

**AI edit of a whole feature**: `update_app` with an instruction prompt (spends credits, runs the platform generator).

**Safety net**: `list_saved_versions` / `restore_saved_version` roll the app back; `compare_current_to_version` shows drift. `restore_*` tools are destructive to current state, confirm before using.

**Domains**: `add_custom_domain` → user creates DNS records from `get_domain_verification` → `check_domain_verification` → `set_primary_domain`. TLS is automatic; check with `get_domain_ssl_status`.

**Secrets and integrations**: `set_secret` for API keys (never hard-code); `list_integrations` / `configure_integration` for platform-supported services.

**Access control**: `set_route_policy` gates pages behind login; `set_entity_policy` controls data access; app members via `invite_app_member` / `update_app_member_role`.

**Media**: `generate_image`, `generate_video` (check `estimate_video_cost` first), `generate_slideshow`. These spend account credits; only use when asked.

## Conventions

- Keep the linked app id in `.finestructure.json` at the project root: `{"app_id": "..."}`.
- Poll jobs with `get_job_status`; generation can take minutes. Do not re-fire a job because polling is slow.
- After publishing, verify with `get_errors` and share the URL from `get_app_links`.
- Auth errors mean the OAuth connection needs a reconnect; credit errors mean the user must top up at https://finestructure.ai/upgrade. Neither is retryable.
