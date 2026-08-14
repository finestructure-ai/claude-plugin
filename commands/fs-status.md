---
description: Show the status, live links and recent errors of a Fine Structure app
argument-hint: [app name, optional when the project is linked]
---

Report the current state of the user's Fine Structure app using the `finestructure` MCP server.

1. Resolve the app: `.finestructure.json` in the project root has `app_id`; otherwise match $ARGUMENTS against `list_apps`; otherwise show the user's apps and ask.
2. Call `get_app_status`, `get_app_links` and `get_errors` for the app.
3. If domains matter to the user, add `list_app_domains` and `get_domain_ssl_status`.
4. Summarize in a few lines: published or not, live URLs, last update, and any runtime errors with the file/line when available. Offer to fix errors if there are any.
