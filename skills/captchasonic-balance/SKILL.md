---
name: captchasonic-balance
description: Check your CaptchaSonic account balance and verify the API key is working. Use when you want to see remaining credits or confirm connectivity.
---

# Check CaptchaSonic Balance

Use the CaptchaSonic MCP `get_balance` tool to check your account credits.

## Prerequisites

The CaptchaSonic MCP server must be connected with a valid `SONIC_API_KEY`. If not set up, use the **captchasonic-setup** skill first.

## Step 1: Call the MCP Tool

Use the `get_balance` MCP tool (no arguments needed — the API key is set via environment variable):

```
Tool: get_balance
Arguments: {}
```

## Step 2: Interpret the Result

The tool returns:
- `balance` — your remaining credits in USD (e.g., `$12.5400`)
- `status` — account status
- `errorId` — `0` means success

## Step 3: Report to the User

Present the balance clearly:
- If balance is healthy: "Your CaptchaSonic balance is **$12.54 USD**."
- If balance is low (under $1.00): "⚠️ Your balance is **$0.32 USD** — consider topping up at [captchasonic.com](https://captchasonic.com)."
- If balance is zero: "❌ Your balance is **$0.00** — add credits at [captchasonic.com](https://captchasonic.com) to continue solving captchas."

## Error Handling

| Error | Meaning | Fix |
|---|---|---|
| "SONIC_API_KEY is not set" | MCP server doesn't have the API key | Re-add the MCP server with `--env SONIC_API_KEY=sonic_xxxx` |
| `errorId: 1` | Invalid API key | Check your key at [dashboard.captchasonic.com](https://captchasonic.com) |
| Connection error | MCP server not running | Run `npx -y @captchasonic/mcp-server` or use the setup skill |

## Fallback: Python SDK

If the MCP server is not available:

```bash
pip install captchasonic
```

```python
from captchasonic import SonicClient
import os

client = SonicClient(api_key=os.environ.get("SONIC_API_KEY", "sonic_xxxx"))
balance = client.get_balance()
print(f"Balance: ${balance:.4f} USD")
client.close()
```

## Fallback: grpcurl

```bash
grpcurl -d '{"apiKey":"sonic_xxxx"}' \
  api.captchasonic.com:443 captchasonic.v1.SonicService/GetBalance
```
