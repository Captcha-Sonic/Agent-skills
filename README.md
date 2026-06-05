# CaptchaSonic Agent Skill — Solve Any CAPTCHA with AI | Claude Code Plugin & MCP Server

> Solve reCAPTCHA, Cloudflare Turnstile, popular captcha, Geetest, AWS WAF, TikTok, and 
> OCR captchas using AI. Install as a Claude Code plugin or MCP server — 
> works with Claude, Cursor, Windsurf, and any MCP-compatible AI agent.

[![npm](https://img.shields.io/npm/v/@captchasonic/mcp-server)](https://www.npmjs.com/package/@captchasonic/mcp-server)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Claude Code](https://img.shields.io/badge/Claude_Code-Plugin-blueviolet)](https://claudemarketplaces.com)
[![MCP](https://img.shields.io/badge/MCP-Server-green)](https://modelcontextprotocol.io)

## Quick Install

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

## Why CaptchaSonic?

- ⚡ **Instant solving** — synchronous results for image captchas, no polling needed
- 🎯 **13+ captcha types** — reCAPTCHA, Turnstile, popular captcha, Geetest, AWS WAF, TikTok, Binance, OCR
- 🔌 **MCP native** — works with Claude Code, Claude Desktop, Cursor, Windsurf, Cline
- 🧩 **Plugin install** — two commands, no cloning or manual setup
- 💰 **Pay per solve** — no subscription, credits start at $1
- 📊 **High accuracy** — AI-powered solving with automatic retry

## MCP Tools & Claude Skills

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

## Supported CAPTCHA Types (13+)

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
| Audio | Speech recognition | Transcribed text |

## Compatible With

| AI Agent | Install Method |
|---|---|
| **Claude Code** | `/plugin install captchasonic` |
| **Claude Desktop** | `claude_desktop_config.json` |
| **Cursor** | MCP server config |
| **Windsurf** | MCP server config |
| **Cline** | MCP server config |
| **Any MCP client** | `npx -y @captchasonic/mcp-server` |

## Installation Guide

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

## FAQ

**What captcha types does CaptchaSonic support?**  
CaptchaSonic supports 13+ captcha types including reCAPTCHA v2/v3, Cloudflare Turnstile, PopularCaptcha, Geetest v3/v4, AWS WAF, TikTok, Binance, and OCR text recognition.

**How do I install CaptchaSonic in Claude Code?**  
Run `/plugin marketplace add captchasonic/captchasonic-skill` then `/plugin install captchasonic`. Then add your API key with `claude mcp add`.

**Does CaptchaSonic work with Cursor and Windsurf?**  
Yes. CaptchaSonic provides a standard MCP server that works with any MCP-compatible AI agent including Claude Code, Claude Desktop, Cursor, Windsurf, and Cline.

**How much does CaptchaSonic cost?**  
CaptchaSonic uses pay-per-solve pricing with no subscription. Check your balance with the `get_balance` tool. Get credits at [captchasonic.com](https://captchasonic.com).

**Is CaptchaSonic an MCP server?**  
Yes. CaptchaSonic provides a fully compliant MCP (Model Context Protocol) server published as `@captchasonic/mcp-server` on npm. It communicates via JSON-RPC over stdio.

## Resources & Documentation

- 🌐 [CaptchaSonic Website](https://captchasonic.com)
- 📦 [MCP Server on npm](https://www.npmjs.com/package/@captchasonic/mcp-server)
- 📖 [SDK Documentation](https://github.com/captchasonic/sonic-sdk)
- 🐛 [Report Issues](https://github.com/captchasonic/captchasonic-skill/issues)

## License

MIT — [CaptchaSonic](https://captchasonic.com)
