# CaptchaSonic Plugin for Claude Code

## What This Plugin Does

This plugin gives you the ability to solve any CAPTCHA using the CaptchaSonic API.
It provides an MCP server with 3 tools and 3 skills for guided workflows.

## MCP Tools Available

| Tool | Description | API Key |
|---|---|---|
| `health_check` | Check if the CaptchaSonic API is online | Not needed |
| `get_balance` | Check account balance in USD | Required |
| `solve_captcha` | Solve any CAPTCHA type from image or URL | Required |

## Setup

The MCP server is auto-configured via `.mcp.json`. Set your API key:

```bash
claude mcp add sonic --env SONIC_API_KEY=sonic_xxxx -- npx -y @captchasonic/mcp-server
```

Get your key at https://captchasonic.com (starts with `sonic_`).

## Skills

- `/captchasonic:captchasonic-solve` — Solve any CAPTCHA (reCAPTCHA, Turnstile, Geetest, OCR, etc.)
- `/captchasonic:captchasonic-balance` — Check account balance
- `/captchasonic:captchasonic-setup` — First-time setup and verification

## Supported CAPTCHA Types

PopularCaptcha, reCAPTCHA v2/v3, Cloudflare Turnstile, Geetest (slide/click/nine),
AWS WAF, TikTok, Binance, OCR, Audio, and generic sliders.

## Solution Types

- `grid` → tile indices [0, 3, 5] (click these tiles)
- `text` → ["abc123"] (type this text)
- `slide` → x=42 (drag 42px right)
- `click` → [{x,y}] (click at coordinates)
- `single` → [true, false] (select true tiles)
