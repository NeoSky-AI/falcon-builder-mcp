# Falcon Builder MCP Server

Build, edit, and publish production AI agents from Claude, Cursor, ChatGPT, VS Code, or any MCP client. The Falcon Builder MCP server lets your assistant inspect executions, edit and duplicate workflows, run them, and publish them in your Falcon Builder workspace.

Available on every plan, including Free.

- **Endpoint:** `https://www.falconbuilder.dev/api/mcp` (Streamable HTTP)
- **Docs:** https://www.falconbuilder.dev/docs/mcp
- **Registry name:** `dev.falconbuilder/falcon-builder`

This is a hosted server. Nothing to install or run locally.

[![smithery badge](https://smithery.ai/badge/mark-cqxq/falcon-builder)](https://smithery.ai/servers/mark-cqxq/falcon-builder)

## Connect

### OAuth clients: claude.ai, Claude Desktop, ChatGPT

No token needed. The client registers itself automatically.

**claude.ai / Claude Desktop**
1. Settings → Connectors → Add custom connector
2. Server URL: `https://www.falconbuilder.dev/api/mcp`
3. Leave OAuth Client ID and Client Secret empty
4. Pick a workspace and the permissions to grant, then approve

**ChatGPT**
Settings → Connectors → Create (turn on Developer Mode if prompted). Same URL, OAuth fields left empty.

### Token clients: Claude Code, Cursor, VS Code, Gemini CLI

Create a token first: **Falcon Builder → Settings → Developer**, name it, choose permissions, and copy it. Tokens are shown once. Each workspace can have up to 10 active tokens.

**Claude Code**
```bash
claude mcp add --transport http falcon-builder \
  https://www.falconbuilder.dev/api/mcp \
  --header "Authorization: Bearer YOUR_TOKEN"
```

**Cursor** (`.cursor/mcp.json`)
```json
{
  "mcpServers": {
    "falcon-builder": {
      "url": "https://www.falconbuilder.dev/api/mcp",
      "headers": { "Authorization": "Bearer YOUR_TOKEN" }
    }
  }
}
```

**VS Code** (`.vscode/mcp.json`). VS Code prompts for the token and stores it securely.
```json
{
  "inputs": [
    {
      "type": "promptString",
      "id": "falcon-token",
      "description": "Falcon Builder API token (Settings > Developer)",
      "password": true
    }
  ],
  "servers": {
    "falcon-builder": {
      "type": "http",
      "url": "https://www.falconbuilder.dev/api/mcp",
      "headers": { "Authorization": "Bearer ${input:falcon-token}" }
    }
  }
}
```

**Gemini CLI** (`~/.gemini/settings.json`). Use `httpUrl`; in Gemini CLI, `url` means SSE.
```json
{
  "mcpServers": {
    "falcon-builder": {
      "httpUrl": "https://www.falconbuilder.dev/api/mcp",
      "headers": { "Authorization": "Bearer YOUR_TOKEN" }
    }
  }
}
```

## Permissions

You choose these when you connect over OAuth or create a token. Read is always included.

| Permission | What it allows |
|---|---|
| Read | Inspect workflows, executions, logs, version history, and interfaces |
| Edit drafts | Change workflow drafts and configure interfaces without touching production |
| Run workflows | Execute workflows with test input. Runs count against your plan limits |
| Publish & rollback | Deploy drafts to production and restore earlier versions |

## Tools

**Read**
`whoami`, `switch_workspace`, `list_agents`, `list_workflows`, `get_workflow`, `get_workflow_definition`, `get_workflow_node`, `validate_workflow`, `list_executions`, `get_execution`, `get_execution_logs`, `list_workflow_versions`, `list_interfaces`, `get_interface`

**Edit drafts**
`create_workflow`, `edit_workflow`, `duplicate_workflow`, `duplicate_agent`, `create_interface`, `update_interface`, `add_interface_access_rule`, `remove_interface_access_rule`, `delete_interface`

**Run workflows**
`run_workflow`, `rerun_executions`

**Publish & rollback**
`publish_workflow`, `rollback_workflow`, `publish_interface`

Every tool declares MCP annotations (read-only, destructive, idempotent, open-world), so clients that honor them can ask before anything changes. `run_workflow` and `rollback_workflow` also require an explicit `confirm: true`.

## Things to ask your assistant

- "Why did the last run of my intake workflow fail? Show me the node that errored."
- "Duplicate my support agent and change its escalation step to email the on-call address."
- "Validate the draft of my lead-routing workflow, then publish it."
- "Roll my onboarding workflow back to the previous version."

## About

Falcon Builder is built by [NeoSky AI](https://neosky.ai).
