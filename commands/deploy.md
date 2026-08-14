---
description: Deploy the current project to Fine Structure and get a live URL
argument-hint: [app name or instructions, e.g. "my-crm" or "update the existing app"]
---

Deploy the user's work to Fine Structure using the `finestructure` MCP server. Fine Structure hosts complete apps: React pages, a managed database (entities), auth, custom domains, secrets and integrations. A deploy always ends with a live URL.

Follow this decision tree:

## 1. Figure out the target app

- If the project root has a `.finestructure.json` file, read `app_id` from it. This is a linked app: you will update it, not create a new one.
- Otherwise call `list_apps` and check whether an app matching this project already exists (by name). If it clearly matches, confirm with the user before writing to it.
- Otherwise this is a first deploy: you will create a new app. Use $ARGUMENTS as the app name if given, else derive a short name from the project.

## 2. First deploy of a fresh idea (no local code to port)

If the user is describing what they want rather than pointing at existing code, call `create_app` with a detailed prompt. Poll `get_job_status` until done, then skip to step 5.

## 3. First deploy of existing local code

Fine Structure apps are React (JSX) pages served by the platform runtime, with data stored in platform entities. Port the local project rather than uploading it blindly:

1. `create_app` with a minimal prompt describing the app shell, wait for it to finish.
2. `get_app_files` and `get_platform_guide` to learn the app structure and platform conventions (pages live under `pages/`, shared code under `components/`).
3. Port each local page/component into the app with `write_app_file`. Keep client-side state; replace any local backend calls with platform entities (`create_entity_schema`, then `entities` APIs from the generated code) or `configure_integration` for external services.
4. Static assets: reference them by URL or inline them; do not try to upload binary files through file-write tools.
5. Store secrets with `set_secret`, never hard-code them.

Write `.finestructure.json` with `{"app_id": "..."}` to the project root so future deploys are updates.

## 4. Updating a linked app

1. `get_app_files` to see the remote state; compare with local changes.
2. For 1-3 file edits use `write_app_file` / `patch_app_file` directly.
3. For larger changes use a change set: `create_change_set`, `add_file_change` per file, `validate_change_set`, then `apply_change_set`. Discard with `discard_change_set` if validation fails.
4. `validate_app` after writing; fix any reported issues before publishing.

## 5. Publish and verify

1. `publish_app` to make the app live.
2. `get_app_links` for the public URL, and `get_errors` to confirm a clean runtime.
3. Show the user the live URL. If the app has a custom domain configured, show that too (`list_app_domains`).

## Rules

- Never call `delete_*`, `remove_*` or `restore_*` tools during a deploy unless the user explicitly asked.
- If a tool fails with an auth error, tell the user to reconnect the Fine Structure server (`/mcp` shows connection status) rather than retrying in a loop.
- If credits run out (payment/credits error), stop and tell the user; do not retry.
- Publishing costs nothing extra; media generation tools spend credits, so only use them when asked.
