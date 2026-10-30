# tlly MCP — client setup

Remote endpoint: **https://tlly.dev/mcp** (Streamable HTTP). Sign in with **OAuth 2.0** when prompted. No API key is required in client config.

For product docs and account setup, see [tlly.dev](https://tlly.dev).

## Claude (claude.ai)

1. Open **Settings → Connectors** (or **Developer** / custom MCP connectors, depending on your plan).
2. Add a **custom connector** (remote / HTTP).
3. Set the server URL to `https://tlly.dev/mcp`.
4. Complete OAuth sign-in to your tlly workspace when the client asks.

## Claude Code

From a terminal:

```bash
claude mcp add --transport http tlly https://tlly.dev/mcp
```

Follow the OAuth flow when you first use the server.

## Cursor

Add an entry to your MCP config (`~/.cursor/mcp.json` or project `.cursor/mcp.json`):

```json
{
  "mcpServers": {
    "tlly": {
      "url": "https://tlly.dev/mcp"
    }
  }
}
```

Restart Cursor or reload MCP, then approve OAuth when connecting.

## ChatGPT

1. Enable **Developer mode** or **Connectors** (per your ChatGPT plan).
2. Add a connector pointing at `https://tlly.dev/mcp`.
3. Authenticate with OAuth when prompted.

## VS Code

Use an MCP extension that supports remote HTTP servers (for example the official MCP extension). Example `mcp.json`:

```json
{
  "servers": {
    "tlly": {
      "type": "http",
      "url": "https://tlly.dev/mcp"
    }
  }
}
```

Exact file location and schema depend on your extension; use `type: http` and the tlly URL above.

## Troubleshooting

- **401 / auth errors:** Run through OAuth again; ensure you are signed into the correct tlly workspace at [tlly.dev](https://tlly.dev).
- **Connection refused / DNS:** Confirm the URL is exactly `https://tlly.dev/mcp` (HTTPS, no trailing path beyond `/mcp`).
- **Tools missing after connect:** Disconnect and reconnect the server so the client refreshes the tool list from the server card.

Questions or bugs with the connector listing: [GitHub Issues](https://github.com/FifthsegmentEng/tlly-mcp/issues).
