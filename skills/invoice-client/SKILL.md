---
name: invoice-client
description: Create and send a FreshBooks invoice. Use when the user wants to bill a client, create an invoice, or charge someone. Triggers on "invoice", "bill", "charge", "send invoice".
---

# Invoice Client

Create and send a FreshBooks invoice through a guided conversation.

## Prerequisites

Ensure the FreshBooks MCP server is connected. If not, tell the user to connect it first.

## Flow

### Step 1: Bootstrap

Call `freshbooks_get_current_user` to get the `account_id` from `business_memberships[0].business.account_id`. Store this for all subsequent calls.

### Step 2: Gather Invoice Details

Ask the user conversationally:
- **Who** to invoice (client name or email)
- **What** to bill for (item/service names, or a custom description)
- **How much** (if not using pre-defined items with set prices)

### Step 3: Resolve Client

Use the `resolve-entity` prompt with `entity_type="client"` and the user's search term.
- If multiple matches, present options and ask the user to pick.
- If no match, offer to create a new client.

### Step 4: Resolve Items

If the user named specific items/services, use the `prepare-invoice-context` prompt to look them up.
If they described custom work, you'll create the invoice with custom line items (no item ID needed — use `description` and `unit_cost` directly).

### Step 5: Preview

Show the user a preview before creating:

```
Invoice Preview
───────────────
To: [Client Name] ([email])

  Line Items:
  1. [Item/Description]    [Qty] x $[Price]  = $[Total]
  2. ...

  Subtotal: $X,XXX.XX
  Tax ([name] [rate]%): $XXX.XX
  Total: $X,XXX.XX
```

### Step 6: Create Invoice (CONFIRM FIRST)

**IMPORTANT: Ask for explicit confirmation before creating.**

Say: "Ready to create this invoice for $[total] to [client name]?"

Only after the user confirms, call `freshbooks_create_invoice` with the gathered parameters.

### Step 7: Send or Save

After creation, ask: "Invoice #[number] created. Would you like to send it to [email] now, or save it as a draft?"

If sending:
**IMPORTANT: Ask for explicit confirmation before sending.**

Say: "Send invoice #[number] to [email]?"

Only after confirmation, call `freshbooks_send_invoice`.

## Error Handling

- If client not found: offer to create a new client
- If item not found: fall back to custom line item
- If invoice creation fails: show the error and suggest corrections
