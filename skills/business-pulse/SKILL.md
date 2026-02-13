---
name: business-pulse
description: Get a plain-English financial health summary of your business. Use when the user asks about business health, financial overview, dashboard, revenue, or how their business is doing. Triggers on "business health", "financial overview", "how am I doing", "dashboard", "business pulse".
---

# Business Pulse

Generate a comprehensive financial health snapshot — fully autonomous (all read-only).

## Prerequisites

Ensure the FreshBooks MCP server is connected.

## Flow

### Step 1: Bootstrap

Call `freshbooks_get_current_user` to get `account_id`, `business_id`, and `business_uuid`.

### Step 2: Determine Date Range

If the user specified a period, use it. Otherwise default to the current calendar month.

For comparison data, also calculate the previous period (e.g., last month if current period is this month).

### Step 3: Pull Reports (Parallel)

All of these are read-only — execute without confirmation:

1. Use `summarize-reports` prompt with current period dates
2. Use `review-outstanding` prompt for AR aging
3. Call `freshbooks_list_expenses` with current period for expense breakdown

Run the report calls in parallel for speed.

### Step 4: Present Summary

Format as a clear business dashboard:

```
Business Pulse — [Business Name]
Period: [Start] to [End]
══════════════════════════════════

Revenue & Profit
  Revenue:    $XX,XXX    (↑/↓ X% vs last period)
  Expenses:   $XX,XXX
  Net Profit: $XX,XXX

Cash Position
  Current Cash: $XX,XXX
  Cash Flow:    +/- $X,XXX this period

Accounts Receivable
  Total Outstanding: $XX,XXX
  Current:     $X,XXX (N invoices)
  0-30 days:   $X,XXX (N invoices)
  31-60 days:  $X,XXX (N invoices)
  60+ days:    $X,XXX (N invoices)  ⚠️

Top Expense Categories
  1. [Category]: $X,XXX
  2. [Category]: $X,XXX
  3. [Category]: $X,XXX
```

### Step 5: Key Takeaways

End with 2-3 actionable insights:
- Flag overdue invoices that need follow-up
- Note unusual expense spikes
- Highlight positive trends

Do NOT ask for confirmation at any point — this is entirely read-only.
