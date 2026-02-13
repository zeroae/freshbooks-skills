---
name: business-pulse
description: Get a plain-English financial health summary of your business. Use when the user asks about business health, financial overview, dashboard, revenue, or how their business is doing. Triggers on "business health", "financial overview", "how am I doing", "dashboard", "business pulse".
context: fork
---

# Business Pulse

Fully autonomous, read-only financial health snapshot. No confirmations needed.

For API patterns and output format, see [references/freshbooks-api-patterns.md](../references/freshbooks-api-patterns.md).

## Flow

1. **Bootstrap** — Call `freshbooks_get_current_user` to get all three IDs.
2. **Date range** — Use user-specified period or default to current calendar month. Calculate previous period for comparison.
3. **Pull reports (parallel, all read-only)**:
   - `summarize-reports` prompt for P&L, balance sheet, cash flow
   - `review-outstanding` prompt for AR aging
   - `freshbooks_list_expenses` for expense breakdown
4. **Present dashboard** — Follow output format from reference. Include: revenue & profit (with period comparison), cash position, AR aging buckets, top expense categories.
5. **Key takeaways** — 2-3 actionable insights: overdue invoices needing follow-up, unusual expense spikes, positive trends.
