# Installing the Reeltopus MCP server

Reeltopus is a remote MCP server; nothing to install or build.

- URL: `https://reeltopus.com/api/mcp`
- Transport: Streamable HTTP
- Auth: OAuth 2.1 (the client opens a Reeltopus sign-in and consent page), or a header
  `Authorization: Bearer rt_live_…` with an API key from https://reeltopus.com/app/settings (API section).

Example client config:

```json
{ "mcpServers": { "reeltopus": { "type": "streamable-http", "url": "https://reeltopus.com/api/mcp" } } }
```

After connecting, call `get_account` to check that it works.
