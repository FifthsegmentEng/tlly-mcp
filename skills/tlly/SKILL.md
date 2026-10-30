---
name: tlly
description: Build tlly dashboards, scheduled data jobs, thresholds, and alerts over MCP. Use when the user wants KPI monitoring, morning checks, shareable metric pages, or recurring fetches from Stripe, Shopify, HubSpot, Google Sheets, or similar sources.
---

# tlly MCP

Connect to the hosted tlly server at `https://tlly.dev/mcp` (OAuth when prompted). Full tool schemas live in the [MCP server card](https://tlly.dev/.well-known/mcp/server-card.json). Read [llms.txt](https://tlly.dev/llms.txt) for data-source constraints.

## Before you invent APIs

1. Call `get_integration_skill` (or read `tlly://skills/{id}`) for the source you use — Stripe, HubSpot, Google Sheets, etc.
2. Call `list_secrets` before jobs that need `{{secret:NAME}}`. Missing secrets: ask the user to add them at [tlly.dev/account](https://tlly.dev/account) (Data sources). Never ask them to paste secrets in chat if they can use the account UI instead.

## Typical workflow

1. **`test_job`** — Dry-run a fetch spec; fix extract/paging until the preview looks right.
2. **`create_job`** — Schedule the job (runs once immediately, then on cron in UTC).
3. **`push_points`** — Write values you computed yourself (upsert by date).
4. **`define_derived`** — Add derived series (% change, spreads, red/yellow/green status) when needed.
5. **`create_dashboard`** — Build panels with thresholds (`red_above`, `yellow_below`, etc.); share the returned URL.
6. **`get_status`** — Check failing jobs, stale panels, and storage issues.
7. **`set_alert_webhook`** / **`set_panel_alert`** / **`set_stale_alert`** — Wire alerts when the user wants email or Slack/Discord webhooks.

## Dashboard and job tips

- Prefer **`update_dashboard`** with `draft: true` for large layout changes; **`publish_dashboard_version`** when the user confirms.
- Keep stable panel **`id`** fields when editing dashboards so panel alerts stay attached.
- Use **`get_usage`** before large backfills or many series.
- For Google Sheets, start from the KPI template linked in `get_integration_skill` for `google-sheets`.
- POST-only analytics (Plausible, PostHog, GA4, Notion DB): use a Sheet or the workspace push address, not a stored-secret job.

## Auth

Prefer OAuth via the MCP client. API keys (`dk_…`) are optional; the user creates them in the members area if needed.
