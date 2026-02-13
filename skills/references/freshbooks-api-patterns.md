# FreshBooks API Patterns

Reference for skills that orchestrate FreshBooks MCP tools.

## ID Systems

FreshBooks uses three different ID types. Get all three from `freshbooks_get_current_user`:

- **account_id** (string, e.g. "zDmNq"): Used for `/accounting/account/{id}/...` endpoints — clients, invoices, expenses, estimates, payments, items, taxes, bills, vendors
- **business_id** (integer, e.g. 373845): Used for `/projects/business/{id}/...` and `/timetracking/business/{id}/...` — projects, time entries
- **business_uuid** (string, UUID format): Used for `/accounting/businesses/{uuid}/...` — reports (P&L, balance sheet, cash flow, chart of accounts, general ledger)

## Bootstrap Flow

Every session should start with `freshbooks_get_current_user` to retrieve:
1. `business_memberships[0].business.account_id` -> for most tools
2. `business_memberships[0].business.id` -> for projects/time tracking
3. `business_memberships[0].business.uuid` -> for reports

## Soft Deletion

Most resources use soft-delete via `vis_state: 1` (not HTTP DELETE). Exceptions: projects and time entries use actual DELETE.

## Pagination

- Parameters: `page` (1-indexed) and `per_page` (max 100)
- Always check `pages` in response to know if more data exists

## Response Envelopes

- Accounting endpoints: `{"response": {"result": {resource_key: data, "page": N, ...}}}`
- Projects/time tracking: `{resource_key: data, "meta": {"page": N, ...}}`

## Output Format

Use this consistent structure for all skill output:

```
[Skill Name] — [Business Name]
════════════════════════════════

[Data sections relevant to the skill]

Actions Taken:
  ✓ [completed action]
  ✓ [completed action]

Next Steps:
  - [suggested follow-up]
  - [suggested follow-up]
```

- Use `$X,XXX.XX` currency formatting
- Flag concerning numbers (net loss, 60+ day overdue)
- Omit "Actions Taken" section for read-only skills
- Omit "Next Steps" if no natural follow-up
