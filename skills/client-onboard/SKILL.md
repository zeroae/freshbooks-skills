---
name: client-onboard
description: Set up a new FreshBooks client with optional project and estimate. Use when the user wants to add a new client, onboard a customer, or set up a new account. Triggers on "new client", "onboard", "add client", "set up client".
---

# Client Onboard

Set up a new client, optionally creating a project and estimate.

For API patterns and output format, see [references/freshbooks-api-patterns.md](../references/freshbooks-api-patterns.md).

## Flow

1. **Bootstrap** — Call `freshbooks_get_current_user` to get `account_id` and `business_id`.
2. **Gather client details** — Name (first/last or organization), email (required), organization.
3. **Create client** — Preview details, then `freshbooks_create_client`. **CONFIRM FIRST.**
4. **Optional: Create project** — Ask: "Create a project for [client]?" If yes, gather title and description. `freshbooks_create_project`. **CONFIRM FIRST.**
5. **Optional: Create estimate** — Ask: "Send an estimate to [client]?" If yes, use `prepare-invoice-context` prompt to resolve items. `freshbooks_create_estimate`. **CONFIRM FIRST.**
6. **Summary** — Client ID, email, project (if created), estimate number (if created).
