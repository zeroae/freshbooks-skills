---
name: month-end-close
description: Guided month-end bookkeeping checklist. Use when the user wants to close the books, do monthly review, or perform month-end accounting. Triggers on "month end", "close the books", "monthly review", "month-end close".
---

# Month-End Close

Walk through a structured month-end bookkeeping checklist.

For API patterns and output format, see [references/freshbooks-api-patterns.md](../references/freshbooks-api-patterns.md).

## Flow

1. **Bootstrap** — Call `freshbooks_get_current_user` to get all IDs. Determine the closing month (default: previous calendar month).
2. **Show checklist** upfront so user knows what to expect:
   - Uncategorized expenses
   - Outstanding invoices
   - Profit & Loss review
   - Tax summary
   - Journal entries
3. **Walk through each item:**

### Uncategorized Expenses
- `freshbooks_list_expenses` filtered to closing month. Flag uncategorized ones.
- Use `categorize-expense` prompt to suggest categories. **CONFIRM** before updating.

### Outstanding Invoices
- Use `review-outstanding` prompt. For invoices due in closing month still unpaid, ask about follow-up or write-off. **CONFIRM** before any write-off.

### Profit & Loss
- `freshbooks_get_profit_and_loss` for closing month. Present key figures, flag unusual items.

### Tax Summary
- `freshbooks_get_tax_summary` for closing month. Show collected vs paid taxes.

### Journal Entries
- `freshbooks_list_journal_entries` for closing month. Ask if adjusting entries needed. **CONFIRM** before creating.

4. **Final summary** — Actions taken, key numbers (revenue, expenses, net profit, tax owed).
