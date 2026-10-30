<p align="center">
  <img src="logo.png" alt="tlly logo" width="128" height="128">
</p>

# tlly MCP

**[tlly](https://tlly.dev)** is a hosted service for **dashboards** and **scheduled data jobs** built for AI agents and teams: pull numbers from Stripe, Shopify, HubSpot, Google Sheets, and more; chart them with thresholds; get morning checks, alerts, and shareable links.

This repository is the **public listing** for tlly’s remote MCP server (metadata, logo, and setup docs). It does **not** contain the hosted service source code.

| | |
| --- | --- |
| **Website** | [https://tlly.dev](https://tlly.dev) |
| **MCP endpoint** | `https://tlly.dev/mcp` |
| **Transport** | Streamable HTTP |
| **Auth** | OAuth 2.0 (sign in with your tlly account; no API key in config) |
| **Support / issues** | [GitHub Issues](https://github.com/FifthsegmentEng/tlly-mcp/issues) |

The MIT [LICENSE](./LICENSE) in this repo applies to **this repository’s documentation and metadata only**, not to the tlly hosted service or its backend.

## Quick setup

1. Add the remote server URL **`https://tlly.dev/mcp`** in your client.
2. Complete **OAuth** when prompted (same account you use at [tlly.dev](https://tlly.dev)).

**Claude (custom connector):** Settings → Connectors → add remote URL `https://tlly.dev/mcp`.

**Claude Code:**

```bash
claude mcp add --transport http tlly https://tlly.dev/mcp
```

**Cursor** (`mcp.json`):

```json
{
  "mcpServers": {
    "tlly": {
      "url": "https://tlly.dev/mcp"
    }
  }
}
```

**ChatGPT:** Developer mode / Connectors → add `https://tlly.dev/mcp`.

**VS Code:** MCP config with `"type": "http"` and `"url": "https://tlly.dev/mcp"`.

Step-by-step notes for each client: **[docs/setup.md](./docs/setup.md)**.

## Tools (30)

One-line summaries below; full parameter schemas and job/dashboard specs are in the live [MCP server card](https://tlly.dev/.well-known/mcp/server-card.json).

### Dashboards

| Tool | Summary |
| --- | --- |
| `create_dashboard` | Create a hosted dashboard; returns its public URL. |
| `update_dashboard` | Replace a dashboard's spec (URL stays the same). |
| `list_dashboard_versions` | List saved dashboard versions and which one is live. |
| `publish_dashboard_version` | Make a saved version of a dashboard the live one (also to roll back). |
| `rotate_dashboard_url` | Issue a new public URL and disable the old one immediately. |
| `list_dashboards` | List dashboards with their URLs. |
| `delete_dashboard` | Delete a dashboard (series data is kept). |

### Series & annotations

| Tool | Summary |
| --- | --- |
| `push_points` | Write dated values to a series (upsert by date). |
| `get_series` | Read the latest points of a series. |
| `list_series` | List all series with type, point counts, and date ranges. |
| `delete_series` | Delete all points of a series, or remove a derived series' definition. |
| `define_derived` | Define a derived series computed from other series when read. |
| `add_annotations` | Attach short dated notes to a series as chart markers. |
| `list_annotations` | Read the latest annotations of a series. |
| `delete_annotations` | Delete annotations on given dates, or all annotations on a series. |

### Jobs & imports

| Tool | Summary |
| --- | --- |
| `test_job` | Dry-run a job spec: fetch, extract and transform, return a preview. |
| `create_job` | Schedule a fetch job; runs once immediately and returns the result. |
| `import_once` | Fetch and save once without scheduling or using a job slot. |
| `update_job` | Replace a job's spec, or pause it with enabled=false. |
| `list_jobs` | List scheduled fetch jobs with their last run status and any error message. |
| `run_job_now` | Run a scheduled fetch job immediately, store any new points, and return the run result. |
| `delete_job` | Delete a job (its series data is kept). |

### Alerts

| Tool | Summary |
| --- | --- |
| `set_alert_webhook` | Set or clear the workspace webhook URL for job and status alerts. |
| `set_panel_alert` | Configure when a dashboard panel sends email or webhook alerts. |
| `set_stale_alert` | Configure when tlly sends stale-data alert email and webhooks because a series stopped updating. |

### Workspace & integrations

| Tool | Summary |
| --- | --- |
| `list_secrets` | List stored secret names and the host each may be sent to. |
| `get_status` | Health check for jobs, stale panels, dashboards, and storage usage. |
| `get_usage` | Report storage usage against quotas and the largest series. |
| `list_integration_skills` | List integration setup playbooks for Stripe, Shopify, Sheets, and more. |
| `get_integration_skill` | Read one integration playbook: credentials, job spec, and gotchas. |

## Pricing

Plans and limits from [tlly.dev](https://tlly.dev#pricing) (start free; upgrade when you need more):

| Plan | Price | Dashboards | Checks | People |
| --- | --- | --- | --- | --- |
| **Free** | $0 | 3 | 5 checks each | Just you |
| **Pro** | $19 / month founding (50 spots left, until 22 Nov 2026, 23:59 Europe/Oslo); then $29 / month or $290 / year | 20 | 200 checks | you + 5 |
| **Business** | $99 / month or $990 / year | 100 | 1,000 checks | you + 20 |

Current founding pricing and availability: see **[tlly.dev](https://tlly.dev)**.

## Registry metadata

[`server.json`](./server.json) describes this server for the [Official MCP Registry](https://modelcontextprotocol.io/) as **`dev.tlly/tlly`** (DNS-verified namespace). Published entry: [registry search for `dev.tlly/tlly`](https://registry.modelcontextprotocol.io/v0/servers?search=dev.tlly/tlly). This repo holds listing metadata and setup docs for that server.
