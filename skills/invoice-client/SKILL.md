---
name: invoice-client
description: Create and send a FreshBooks invoice. Use when the user wants to bill a client, create an invoice, or charge someone. Triggers on "invoice", "bill", "charge", "send invoice".
---

# Invoice Client

Create and send a FreshBooks invoice through a guided conversation.

For API patterns (ID systems, pagination), see [references/freshbooks-api-patterns.md](../references/freshbooks-api-patterns.md).

## Flow

1. **Bootstrap** — Call `freshbooks_get_current_user` to get IDs (see reference).
2. **Gather details** — Ask who to invoice, what for, and how much.
3. **Resolve client** — Use `resolve-entity` prompt with `entity_type="client"`. If no match, offer to create one.
4. **Resolve items** — Use `prepare-invoice-context` prompt for named items. For custom work, use `description` + `unit_cost` directly (no item ID needed).
5. **Preview** — Show client, line items, taxes, total. Get explicit confirmation.
6. **Create** — `freshbooks_create_invoice`. **CONFIRM FIRST.**
7. **Send or save** — Ask whether to send now or save as draft. If sending, call `freshbooks_send_invoice`. **CONFIRM FIRST.**

## Confirmation Gates

- Creating the invoice: "Ready to create this invoice for $[total] to [client]?"
- Sending the invoice: "Send invoice #[number] to [email]?"
