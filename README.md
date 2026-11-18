<p align="center">
  <img src="assets/icon-128.png" width="96" height="96" alt="Highseat">
</p>

# Highseat MCP server

[Highseat](https://highseat.com) is web analytics for your websites. Its MCP server lets an AI assistant read a site's traffic and answer questions such as "What were my top referrers last week for example.com?". A connection that may make changes can also add notes, goals, funnels and saved reports, and change a site's settings.

```text
https://mcp.highseat.com/mcp
```

The server is hosted by Highseat. This repository holds only the files that clients read to install it: plugin manifests, skills and icons. You need a Highseat account with at least one site.

<p>
  <a href="https://cursor.com/install-mcp?name=highseat&config=eyJ1cmwiOiJodHRwczovL21jcC5oaWdoc2VhdC5jb20vbWNwIn0="><img src="https://cursor.com/deeplink/mcp-install-dark.svg" height="28" alt="Add to Cursor"></a>
  <a href="https://vscode.dev/redirect/mcp/install?name=highseat&config=%7B%22type%22%3A%22http%22%2C%22url%22%3A%22https%3A%2F%2Fmcp.highseat.com%2Fmcp%22%7D"><img src="assets/add-to-vscode.svg" height="28" alt="Add to VS Code"></a>
  <a href="https://kiro.dev/launch/mcp/add?name=highseat&config=%7B%22url%22%3A%22https%3A%2F%2Fmcp.highseat.com%2Fmcp%22%2C%22oauthScopes%22%3A%5B%22read%22%2C%22offline_access%22%5D%7D"><img src="https://kiro.dev/images/add-to-kiro.svg" height="28" alt="Add to Kiro"></a>
  <a href="https://replit.com/integrations?mcp=eyJkaXNwbGF5TmFtZSI6IkhpZ2hzZWF0IiwiYmFzZVVybCI6Imh0dHBzOi8vbWNwLmhpZ2hzZWF0LmNvbS9tY3AifQ=="><img src="https://replit.com/badge?caption=Add%20to%20Replit" height="28" alt="Add to Replit"></a>
</p>

## Sign in

Every client below connects through OAuth, so there is no key to copy. The client opens Highseat in your browser. You sign in, pick a workspace, the sites the client may reach and whether it may make changes, and allow it. A connection reads by default. An owner or admin of the workspace can also let it make changes.

To disconnect a client, open **Workspace settings › API keys** in Highseat. Connected apps are listed under the keys.

## Set up your client

### Claude

On claude.ai or in Claude Desktop, add a custom connector and paste the server URL. Claude opens the sign-in page.

### Claude Code

Install the plugin, which adds the server and the skills below:

```bash
claude plugin marketplace add highseat/mcp
claude plugin install highseat@highseat
```

Or add only the server:

```bash
claude mcp add --transport http highseat https://mcp.highseat.com/mcp
```

Then run `/mcp` in Claude Code and pick `highseat` to sign in.

### ChatGPT

Open [Plugins](https://chatgpt.com/plugins), select the plus button, then **Add custom MCP server**. Enter a name, paste the server URL under **Connection** and set up authentication. Then select **I understand and want to continue** and **Create as a plugin**. Type `@` in a new conversation and pick Highseat to use it. Your plan and your workspace's policies decide whether you can add a custom server.

### Codex

```bash
codex mcp add highseat --url https://mcp.highseat.com/mcp
```

Codex opens the sign-in page. If it does not, run `codex mcp login highseat`.

### Cursor

Use the **Add to Cursor** button above, or add the server to `~/.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "highseat": { "url": "https://mcp.highseat.com/mcp" }
  }
}
```

Then sign in from Cursor's MCP settings.

### VS Code

Use the **Add to VS Code** button above, or add the server to `.vscode/mcp.json`:

```json
{
  "servers": {
    "highseat": { "type": "http", "url": "https://mcp.highseat.com/mcp" }
  }
}
```

VS Code asks you to sign in when it starts the server.

### Kiro

Use the **Add to Kiro** button above, or import this repository as a power: **Powers › Add Custom Power › Import power from GitHub**, with `https://github.com/highseat/mcp`. To add only the server, put this in `~/.kiro/settings/mcp.json`:

```json
{
  "mcpServers": {
    "highseat": {
      "url": "https://mcp.highseat.com/mcp",
      "oauthScopes": ["read", "offline_access"]
    }
  }
}
```

Keep `oauthScopes`. Without it Kiro asks for `openid`, `email` and `profile`, which Highseat does not grant, and sign-in fails.

### Replit

Use the **Add to Replit** button above, or open **Integrations** in Replit, add a custom MCP server and paste the server URL. Sign in when Replit asks.

### Gemini CLI

```bash
gemini extensions install https://github.com/highseat/mcp
```

Then run `/mcp auth highseat` in Gemini CLI to sign in.

### Other clients

Any client that speaks Streamable HTTP and supports OAuth needs only the server URL. A client that cannot sign in can send a Highseat API key as `Authorization: Bearer hs_live_…`. The [MCP docs](https://highseat.com/docs/api/mcp) explain how to create one.

## Tools

Every connection lists the tools that read:

| Tool | What it answers |
| --- | --- |
| `listSites` | The sites the connection reaches. Every other site tool takes one of them as `site` |
| `listSiteOverviews` | Recent traffic for every site at once |
| `getSite` | One site's settings |
| `getInstallStatus` | Whether the site's tracker works: first and last event, refused hosts, filtered events |
| `getStats` | Every report on the dashboard, picked by `report`: overview, timeseries, realtime, pages, sources, devices, locations, campaigns, events, properties, goals, revenue, products, attribution, bots and MCP tool calls |
| `getFilterValues`, `getCatalog` | The values a filter takes, and the pages and events a site has ever sent |
| `listNotes`, `listGoals`, `listFunnels`, `listReports` | A site's chart notes, goals, saved funnels and saved reports |
| `getFunnel`, `getFunnelPreview` | A saved funnel's conversion, or a funnel's steps checked before saving |
| `getReportResults` | A saved report's figures |
| `getUsage` | The workspace's event usage |

A connection that may make changes also lists these:

| Tool | What it does |
| --- | --- |
| `saveNote`, `saveGoal`, `saveFunnel`, `saveReport` | Creates a row, or updates the row whose `id` the call passes |
| `saveSite` | Creates a site, or updates the site the call passes as `site` |
| `deleteNote`, `deleteGoal`, `deleteFunnel`, `deleteReport` | Deletes a row |

Each tool call counts against the hourly limits of the connection and the workspace. The [MCP docs](https://highseat.com/docs/api/mcp) cover the limits, errors and each tool's parameters.

## Skills

The plugins in this repository add three skills, for clients that load skills (Claude Code, Cursor, Kiro and Gemini CLI):

- `weekly-traffic-review`: the last seven days of a site against the week before, with top pages, sources, goals and notes.
- `install-and-verify`: installs the tracker on a site and checks with `getInstallStatus` that events arrive.
- `explain-traffic-change`: finds the source, page or country behind a sudden rise or drop.

## Files

| File | Read by |
| --- | --- |
| `.claude-plugin/plugin.json`, `.claude-plugin/marketplace.json` | Claude Code |
| `.cursor-plugin/plugin.json` | Cursor |
| `plugin.json`, `mcp.json` | Kiro, and other clients that read the [Agent Plugins](https://agent-plugins.org) format |
| `gemini-extension.json` | Gemini CLI |
| `skills/` | Every client above |
| `assets/` | The Highseat icon at 512, 400 and 128 pixels, and the VS Code install button |

## Privacy and support

Highseat's [privacy policy](https://highseat.com/privacy) and [terms](https://highseat.com/terms) apply to the MCP server. A connection reaches only the workspace and the sites you pick when you sign in.

- Product page: https://highseat.com/mcp
- Documentation: https://highseat.com/docs/api/mcp
- Support: support@highseat.com

Issues are turned off on this repository. Write to support@highseat.com with any question or problem.

## License

The files in this repository are released under the [MIT License](LICENSE). The license does not cover the Highseat name or icon.
