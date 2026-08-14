---
description: Connect a custom domain to a Fine Structure app
argument-hint: <domain, e.g. app.example.com>
---

Connect the domain in $ARGUMENTS to the user's Fine Structure app using the `finestructure` MCP server.

1. Resolve the app (`.finestructure.json`, else `list_apps`, else ask).
2. `add_custom_domain` with the domain.
3. `get_domain_verification` and show the user the exact DNS records they must create, in a copy-paste friendly table.
4. Once the user says DNS is set, `check_domain_verification`. If verification passes, confirm with `get_domain_ssl_status` that a certificate was issued.
5. Ask whether this should be the primary domain; if yes, `set_primary_domain`, and offer `configure_domain_redirects` for the www/apex twin.

DNS propagation can take minutes to hours; if verification fails right after the records were added, say so and suggest retrying later instead of looping.
