# CaptchaSonic Skill for Claude & AI Agents

> 🧩 **Install this skill** to give your AI agent the ability to solve any CAPTCHA using the [CaptchaSonic](https://captchasonic.com) API.

[![npm](https://img.shields.io/npm/v/@captchasonic/mcp-server)](https://www.npmjs.com/package/@captchasonic/mcp-server)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)

## Install

### Claude Code Plugin (recommended)

```
/plugin marketplace add captchasonic/captchasonic-skill
/plugin install captchasonic
```

Then set your API key:

```bash
claude mcp add sonic --env SONIC_API_KEY=sonic_xxxx -- npx -y @captchasonic/mcp-server
```

### Alternative: One-liner via npx

```bash
npx skills add https://github.com/captchasonic/captchasonic-skill
claude mcp add sonic --env SONIC_API_KEY=sonic_xxxx -- npx -y @captchasonic/mcp-server
```

Get your API key at [captchasonic.com](https://captchasonic.com) (starts with `sonic_`).

## What You Get

Three MCP tools available to your AI agent:

| Tool | Description |
|---|---|
| `health_check` | Verify the CaptchaSonic API is online |
| `get_balance` | Check your account balance in USD |
| `solve_captcha` | Submit a CAPTCHA image and get the solution |

Three Claude skills for guided workflows:

| Skill | Description |
|---|---|
| `captchasonic-setup` | First-time MCP server installation and verification |
| `captchasonic-solve` | Solve any CAPTCHA type with step-by-step guidance |
| `captchasonic-balance` | Check account balance and credit status |

## Supported CAPTCHA Types

| Type | Category | Solution |
|---|---|---|
| PopularCaptcha | Image grid | Tile indices |
| reCAPTCHA v2 | Image grid | Tile indices |
| reCAPTCHA v3 | Token | Token string |
| Cloudflare Turnstile | Token | Token string |
| Cloudflare Challenge | Token | Token string |
| Geetest v3/v4 | Slide/Click/Grid | Offset / Coordinates |
| AWS WAF | Image grid | Tile indices |
| TikTok | Rotation/Click | Offset / Coordinates |
| Binance | Slide puzzle | Pixel offset |
| OCR | Text recognition | Recognized text |

## Installation

### Claude Code Plugin (recommended)

Inside Claude Code, run:

```
/plugin marketplace add captchasonic/captchasonic-skill
/plugin install captchasonic
```

This installs skills + auto-configures the MCP server. Then set your API key:

```bash
claude mcp add sonic --env SONIC_API_KEY=sonic_xxxx -- npx -y @captchasonic/mcp-server
```

### npx skills

```bash
npx skills add https://github.com/captchasonic/captchasonic-skill
claude mcp add sonic --env SONIC_API_KEY=sonic_xxxx -- npx -y @captchasonic/mcp-server
```

### Claude Desktop

Add to your `claude_desktop_config.json`:

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

### MCP Server Only (no skills)

```bash
claude mcp add sonic --env SONIC_API_KEY=sonic_xxxx -- npx -y @captchasonic/mcp-server
```

## Repository Structure

```
captchasonic-skill/
├── SKILL.md                              # Main skill entry point
├── CLAUDE.md                             # Context for Claude Code
├── .mcp.json                             # Auto-configures MCP server on install
├── .claude-plugin/
│   ├── plugin.json                       # Plugin metadata
│   └── marketplace.json                  # Marketplace listing (auto-crawled)
├── skills/
│   ├── captchasonic-solve/SKILL.md       # Solve any CAPTCHA type
│   ├── captchasonic-balance/SKILL.md     # Check account balance
│   └── captchasonic-setup/SKILL.md       # First-time setup guide
├── references/
│   ├── mcp-setup.md                      # Detailed MCP installation
│   ├── captcha-types.md                  # All supported types + parameters
│   ├── solution-types.md                 # Solution format interpretation
│   └── troubleshooting.md               # Common errors + fixes
├── examples/
│   ├── solve-recaptcha.md                # reCAPTCHA grid example
│   ├── solve-turnstile.md                # Cloudflare Turnstile example
│   └── solve-ocr.md                      # OCR text recognition example
├── README.md
└── LICENSE
```

## Environment Variables

| Variable | Required | Default |
|---|---|---|
| `SONIC_API_KEY` | Yes | — |
| `SONIC_BASE_URL` | No | `https://api.captchasonic.com` |

## Links

- 🌐 [CaptchaSonic Website](https://captchasonic.com)
- 📦 [MCP Server on npm](https://www.npmjs.com/package/@captchasonic/mcp-server)
- 📖 [SDK Documentation](https://github.com/captchasonic/sonic-sdk)
- 🐛 [Report Issues](https://github.com/captchasonic/captchasonic-skill/issues)

## License

MIT — [CaptchaSonic](https://captchasonic.com)
