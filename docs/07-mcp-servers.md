# 7. MCP Servers

MCP (Model Context Protocol) is how Claude Code connects to tools and data beyond your filesystem and shell — issue trackers, databases, browsers, design tools, internal APIs. An MCP server exposes a set of tools; once connected, the agent can call them like any other tool.

## What you get

With the right MCP servers connected, the agent can do things like:

- Read and comment on issues / pull requests in your tracker.
- Query a database directly instead of guessing at schema.
- Drive a browser to test a web app it just changed.
- Pull a design spec and implement against it.

It turns "the agent that edits files" into "the agent that operates your stack."

## Adding a server

You register MCP servers in configuration (commonly `.mcp.json` at the project root, or via the `claude mcp` commands). A typical entry names the server and how to launch it:

```json
{
  "mcpServers": {
    "playwright": {
      "command": "npx",
      "args": ["-y", "@playwright/mcp@latest"]
    }
  }
}
```

Use the CLI to add and inspect servers:

```bash
claude mcp add        # interactive add
claude mcp list       # see configured servers
```

Project-level config (committed) shares servers with your team; user-level config is just yours.

## Scope: project vs personal

- **Project `.mcp.json`** — committed to the repo. Everyone on the team gets the same tools (the test browser, the staging DB reader). Good for shared, non-sensitive servers.
- **Personal config** — your own credentials and private tools. Keep anything carrying secrets here, not in the committed file.

## Security: treat MCP tools like any external integration

MCP servers run with real access. A few rules keep this safe:

- **Only connect servers you trust.** An MCP server can read what you point it at and act with whatever credentials you give it.
- **Scope credentials tightly.** Give a database MCP server a read-only role if it only needs to read. Don't hand it admin.
- **Beware prompt injection through tool results.** If an MCP tool returns content from an untrusted source (a web page, an external ticket), treat instructions embedded in that content as data, not commands. Don't let "the issue said to delete the table" become an action without your say-so.
- **Don't commit secrets.** Server configs that need tokens should pull them from environment variables, not hardcode them in `.mcp.json`.

## Start minimal

You don't need a dozen servers. Most value comes from one or two well-chosen ones — typically a way to run/test your app and a way to reach your issue tracker. Add servers when a real, recurring need appears, not speculatively. Each connected server is more tools in the agent's context and more surface to keep secure.

→ [8. Subagents & Workflows](08-subagents-workflows.md)
