---
name: log-and-bill
description: Convert tracked time entries into an invoice. Use when the user wants to bill for their time, invoice time entries, or convert tracked hours to an invoice. Triggers on "bill for time", "invoice time", "convert time to invoice".
---

# Log and Bill

Convert tracked time entries into a FreshBooks invoice.

## Prerequisites

Ensure the FreshBooks MCP server is connected.

## Flow

### Step 1: Bootstrap

Call `freshbooks_get_current_user` to get `account_id` and `business_id`.

### Step 2: Identify Time Entries

Ask the user:
- Which **project** (by name — resolve using `resolve-entity` with `entity_type="project"`)
- What **date range** (default: current month)

### Step 3: Fetch Unbilled Time

Use the `review-unbilled-time` prompt to fetch and summarize unbilled time entries.

Present:
```
Unbilled Time Summary
─────────────────────
Project: [Name]
Period: [start] to [end]

  [Date]  [Duration]  [Service]     [Note]
  Jan 15  2.5 hrs     Consulting    Client meeting + follow-up
  Jan 16  4.0 hrs     Development   Feature implementation
  ...

Total: [X] hours across [Y] entries
Estimated value: $[amount] (at $[rate]/hr)
```

### Step 4: Select Entries

Ask: "Include all entries, or would you like to exclude any?"

If the user wants to exclude some, let them pick by number.

### Step 5: Resolve Client

The project should be linked to a client. If not, ask who to invoice and resolve via `resolve-entity`.

### Step 6: Preview Invoice

Show preview with time entries grouped as line items.

### Step 7: Create Invoice (CONFIRM FIRST)

**IMPORTANT: Ask for explicit confirmation before creating.**

Say: "Create invoice for [X] hours ($[amount]) to [client]?"

Call `freshbooks_create_invoice` after confirmation.

### Step 8: Send or Save

Same as invoice-client Step 7.
