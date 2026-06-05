# MCP Server Setup Guide

## What is the CaptchaSonic MCP Server?

The `@captchasonic/mcp-server` npm package is a Model Context Protocol (MCP) server that exposes three tools to AI agents:

| Tool | Description | API Key |
|---|---|---|
| `health_check` | Check if CaptchaSonic API is healthy and get server version | Not needed |
| `get_balance` | Get account balance in USD | Required |
| `solve_captcha` | Submit a CAPTCHA image for solving | Required |

The server communicates via **stdio** (standard input/output) using the MCP JSON-RPC protocol.

## Installation Methods

### Method 1: One-Line Install (Claude Code CLI)

```bash
claude mcp add sonic --env SONIC_API_KEY=sonic_xxxx -- npx -y @captchasonic/mcp-server
```

This tells Claude Code to:
1. Download `@captchasonic/mcp-server` from npm (via `npx`)
2. Start it as a subprocess
3. Pass `SONIC_API_KEY` as an environment variable
4. Register it under the name `sonic`

### Method 2: Global Install

```bash
npm install -g @captchasonic/mcp-server
claude mcp add sonic --env SONIC_API_KEY=sonic_xxxx -- sonic-mcp
```

Faster startup (no `npx` download each time).

### Method 3: Claude Desktop Config

Edit your Claude Desktop configuration file:

| OS | Config Path |
|---|---|
| macOS | `~/Library/Application Support/Claude/claude_desktop_config.json` |
| Windows | `%APPDATA%\Claude\claude_desktop_config.json` |
| Linux | `~/.config/Claude/claude_desktop_config.json` |

Add to the `mcpServers` section:

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

Restart Claude Desktop after saving.

### Method 4: Antigravity IDE

Add an MCP server in your Antigravity settings:
- **Command**: `npx`
- **Args**: `-y @captchasonic/mcp-server`
- **Environment**: `SONIC_API_KEY=sonic_xxxx`

## Environment Variables

| Variable | Required | Default | Description |
|---|---|---|---|
| `SONIC_API_KEY` | Yes (for solve/balance) | — | Your API key from [captchasonic.com](https://captchasonic.com) |
| `SONIC_BASE_URL` | No | `https://api.captchasonic.com` | Override the API endpoint |

## Verifying the Setup

After installation, test with these commands in order:

1. **Health check** (no API key needed):
   ```
   Tool: health_check
   ```
   Expected: `healthy: true, version: x.x.x`

2. **Balance check** (needs API key):
   ```
   Tool: get_balance
   ```
   Expected: `balance: $XX.XXXX, status: ok, errorId: 0`

3. **Test solve** (needs API key + image):
   ```
   Tool: solve_captcha
   Arguments: { type: "ocr", question: "What text is in this image?", image_url: "..." }
   ```

## Removing the MCP Server

```bash
claude mcp remove sonic
```

Or remove the `sonic` entry from `claude_desktop_config.json` and restart.
