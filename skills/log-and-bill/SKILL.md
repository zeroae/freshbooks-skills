---
name: log-and-bill
description: Convert tracked time entries into an invoice. Use when the user wants to bill for their time, invoice time entries, or convert tracked hours to an invoice. Triggers on "bill for time", "invoice time", "convert time to invoice".
---

# Log and Bill

Convert tracked time entries into a FreshBooks invoice.

For API patterns (ID systems, pagination), see [references/freshbooks-api-patterns.md](../references/freshbooks-api-patterns.md).

## Flow

1. **Bootstrap** — Call `freshbooks_get_current_user` to get `account_id` and `business_id`.
2. **Identify project** — Ask which project and date range (default: current month). Resolve project name via `resolve-entity` prompt with `entity_type="project"`.
3. **Fetch unbilled time** — Use `review-unbilled-time` prompt. Show entries grouped by project with hours, date range, estimated value.
4. **Select entries** — Ask: "Include all entries, or exclude any?" Let user pick by number.
5. **Resolve client** — Get client from project link. If unlinked, resolve via `resolve-entity`.
6. **Preview** — Show time entries as line items with totals.
7. **Create invoice** — `freshbooks_create_invoice`. **CONFIRM FIRST:** "Create invoice for [X] hours ($[amount]) to [client]?"
8. **Send or save** — Same as invoice-client step 7.
