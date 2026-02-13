---
name: expense-capture
description: Quickly log a FreshBooks expense with smart category suggestions. Use when the user wants to record an expense, log a purchase, or track spending. Triggers on "expense", "I spent", "receipt", "I paid", "log expense".
---

# Expense Capture

Quickly log an expense with smart category suggestion.

For API patterns and output format, see [references/freshbooks-api-patterns.md](../references/freshbooks-api-patterns.md).

## Flow

1. **Bootstrap** — Call `freshbooks_get_current_user` to get `account_id`.
2. **Extract details** from user's message — parse amount, description, vendor, date (default: today). Example: "I spent $45 on lunch with a client yesterday" gives amount, description, and date.
3. **Suggest category** — Use `categorize-expense` prompt. Present suggestion and ask if it's right. If not, show full category list.
4. **Preview** — Amount, vendor, category, date.
5. **Create** — `freshbooks_create_expense`. **CONFIRM FIRST:** "Create this expense for $[amount]?"
6. **Done** — Show confirmation with expense ID. Ask: "Anything else to log?"
