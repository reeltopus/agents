# Reeltopus for AI agents

[![mcptry](https://mcptry.com/badge/com.reeltopus/mcp)](https://mcptry.com/servers/com.reeltopus/mcp)

Reeltopus turns long videos into captioned vertical clips, exports them and posts them. It works with Claude,
ChatGPT, Cursor and any MCP client. This repo holds the install files for agent tools; the server itself is
remote: `https://reeltopus.com/api/mcp` (Streamable HTTP, OAuth sign-in or an API key).

Docs: https://reeltopus.com/docs/api/mcp · Support: support@reeltopus.com ·
Privacy: https://reeltopus.com/privacy

## Install

[![Add to Cursor](https://cursor.com/deeplink/mcp-install-dark.svg)](https://cursor.com/en/install-mcp?name=reeltopus&config=eyJ1cmwiOiJodHRwczovL3JlZWx0b3B1cy5jb20vYXBpL21jcCJ9)
[![Install in VS Code](https://img.shields.io/badge/VS_Code-Install_Reeltopus-0098FF?logo=visualstudiocode)](https://insiders.vscode.dev/redirect/mcp/install?name=reeltopus&config=%7B%22type%22%3A%22http%22%2C%22url%22%3A%22https%3A%2F%2Freeltopus.com%2Fapi%2Fmcp%22%7D)

- **Claude Code** (plugin: the server plus two workflow skills):
  ```
  /plugin marketplace add reeltopus/agents
  /plugin install reeltopus@reeltopus
  ```
  Then run `/mcp`, pick reeltopus and sign in. Or add only the server:
  `claude mcp add --transport http reeltopus https://reeltopus.com/api/mcp`
- **Claude** (claude.ai, Desktop, mobile): Customize → Connectors → Add → Add custom connector →
  `https://reeltopus.com/api/mcp`
- **ChatGPT**: chatgpt.com/plugins → + → Add custom MCP server → Public endpoint, the same URL, OAuth.
- **Cursor**: the button above, or `~/.cursor/mcp.json`:
  `{ "mcpServers": { "reeltopus": { "url": "https://reeltopus.com/api/mcp" } } }`
- **Gemini CLI**: `gemini extensions install https://github.com/reeltopus/agents`
- **VS Code CLI**: `code --add-mcp '{"name":"reeltopus","type":"http","url":"https://reeltopus.com/api/mcp"}'`
- **Anything else**: see [llms-install.md](llms-install.md).

## One line in Claude Code

```
/reeltopus:clip-and-schedule
```

or just say "clip my latest video and schedule the best one". Claude finds your newest video (or uploads a file
you name), waits for the clips, picks the best one with the packaging that wins on your account, exports it and
schedules it after you confirm. Arguments are optional: `/reeltopus:clip-and-schedule ~/Movies/ep12.mp4 youtube
tomorrow 9am`.

The server also offers these as MCP prompts in any client: `clip_and_schedule`, `best_clips`, `clip_new_video`
and `post_as_series` (in Claude Code: `/mcp__reeltopus__clip_and_schedule`).

## What it can do

Check hours, upload or find videos, read ranked clips with transcripts, edit captions, trims, B-roll and looks,
export MP4s, caption files and editor timelines, and post or schedule to connected YouTube and LinkedIn
accounts (for other platforms, download the MP4). Agents ask before anything that posts or overwrites.
