---
name: captchasonic-setup
description: Set up the CaptchaSonic MCP server for the first time. Installs the server, configures environment variables, and verifies connectivity. Use when a user wants to start using CaptchaSonic with their AI agent.
---

# CaptchaSonic MCP Server Setup

Set up the CaptchaSonic MCP server so your AI agent can solve CAPTCHAs.

## Step 1: Check Prerequisites

Verify Node.js is installed (v18+):
```bash
node --version
```

If not installed, the user needs to install Node.js from [nodejs.org](https://nodejs.org/).

## Step 2: Get the API Key

Ask the user for their CaptchaSonic API key. It starts with `sonic_`.

If they don't have one: "Register for free at [captchasonic.com](https://captchasonic.com) to get your API key."

## Step 3: Add the MCP Server

### Option A: Claude Code CLI (recommended)

```bash
claude mcp add sonic --env SONIC_API_KEY=sonic_xxxx -- npx -y @captchasonic/mcp-server
```

Replace `sonic_xxxx` with the user's actual API key.

### Option B: Global Install + CLI

```bash
npm install -g @captchasonic/mcp-server
claude mcp add sonic --env SONIC_API_KEY=sonic_xxxx -- sonic-mcp
```

### Option C: Claude Desktop

Edit the Claude Desktop config file.

**macOS:** `~/Library/Application Support/Claude/claude_desktop_config.json`
**Windows:** `%APPDATA%\Claude\claude_desktop_config.json`

Add this to the `mcpServers` section:

```json
{
  "mcpServers": {
    "sonic": {
      "command": "npx",
      "args": ["-y", "@captchasonic/mcp-server"],
      "env": {
        "SONIC_API_KEY": "sonic_xxxx"
      }
    }
  }
}
```

Then restart Claude Desktop.

### Option D: Antigravity IDE

Add MCP server configuration in your Antigravity settings pointing to:
```
npx -y @captchasonic/mcp-server
```
with environment variable `SONIC_API_KEY=sonic_xxxx`.

## Step 4: Verify Health Check

After adding the server, run a health check to confirm connectivity:

```
Tool: health_check
Arguments: {}
```

Expected response:
```
healthy: true
version: x.x.x
```

If the health check fails:
- Check your internet connection
- Verify Node.js is installed and `npx` is available
- Try running `npx -y @captchasonic/mcp-server` manually to see errors

## Step 5: Verify API Key

Run a balance check to confirm the API key is valid:

```
Tool: get_balance
Arguments: {}
```

Expected response:
```
balance: $XX.XXXX
status: ok
errorId: 0
```

If `errorId` is `1`: the API key is invalid. Double-check it at [captchasonic.com](https://captchasonic.com).

## Step 6: Done!

The CaptchaSonic MCP server is now connected. The agent has access to:

- `health_check` — verify server is online
- `get_balance` — check account credits
- `solve_captcha` — solve any CAPTCHA type

Try it: *"Solve this captcha image"* and provide an image path or URL.

## Troubleshooting

Read `references/troubleshooting.md` for common issues and fixes.
