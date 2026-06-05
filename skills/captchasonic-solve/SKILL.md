---
name: captchasonic-solve
description: Solve a CAPTCHA using the CaptchaSonic MCP server. Supports all types — PopularCaptcha, reCAPTCHA, Geetest, OCR, AWS WAF, TikTok, Binance, Cloudflare Turnstile, and more. Provide an image (path, URL, or base64) and captcha type.
---

# Solve a CAPTCHA with CaptchaSonic

Use the CaptchaSonic MCP `solve_captcha` tool to solve any supported CAPTCHA type.

## Prerequisites

The CaptchaSonic MCP server must be connected. If not set up yet, read `references/mcp-setup.md` or use the **captchasonic-setup** skill first.

## Step 1: Identify the CAPTCHA Type

Determine the correct `type` parameter from the user's request:

| User Says | `type` Parameter | Category |
|---|---|---|
| "PopularCaptcha grid" / "select all X" | `PopularCaptchaImage` | Image (sync) |
| "reCAPTCHA v2 grid" | `PopularCaptchaImage` | Image (sync) |
| "Geetest slide" | `geetest_slide` | Image (sync) |
| "Geetest click" | `geetest_click` | Image (sync) |
| "Geetest nine-grid" | `geetest_nine` | Image (sync) |
| "OCR" / "text captcha" | `ocr` | Image (sync) |
| "AWS WAF" | `aws` | Image (sync) |
| "TikTok" | `tiktok` | Image (sync) |
| "Binance slide" | `binance` | Image (sync) |
| "Cloudflare Turnstile" | `turnstile` | Token (async) |
| "reCAPTCHA v2 token" | `recaptcha_v2` | Token (async) |
| "reCAPTCHA v3 token" | `recaptcha_v3` | Token (async) |

## Step 2: Gather Required Information

**For image-based CAPTCHAs (sync):**
- `type` — the captcha type from the table above
- `question` — the challenge text shown to the user (e.g., "Select all traffic lights")
- `image_base64` — base64-encoded PNG/JPEG image bytes
  - OR `image_url` — public URL of the captcha image

**For token-based CAPTCHAs (async):**
- `type` — the captcha type (e.g., `turnstile`)
- `question` — can be empty or describe the task
- `website_url` — the page URL where the captcha appears

## Step 3: Call the MCP Tool

Use the `solve_captcha` MCP tool with the gathered parameters.

**Example for image captcha:**
```
Tool: solve_captcha
Arguments:
  type: "PopularCaptchaImage"
  question: "Select all traffic lights"
  image_base64: "<base64 encoded image>"
```

**Example for token captcha:**
```
Tool: solve_captcha
Arguments:
  type: "turnstile"
  question: "Cloudflare Turnstile"
  website_url: "https://example.com"
```

**If the user provides a file path**, read the image and base64-encode it:
```bash
base64 -i /path/to/captcha.png
```

**If the user provides a URL**, pass it as `image_url`:
```
Tool: solve_captcha
Arguments:
  type: "PopularCaptchaImage"
  question: "Select all traffic lights"
  image_url: "https://example.com/captcha.png"
```

## Step 4: Interpret the Solution

The MCP tool returns a JSON result. Interpret based on the `type` field:

| Solution Type | Meaning | Example |
|---|---|---|
| `grid` | Click tiles at these 0-indexed positions (left→right, top→bottom) | `objects: [1, 3, 5]` → click tiles 2nd, 4th, 6th |
| `text` | Type this text into the captcha field | `texts: ["abc123"]` |
| `slide` | Drag the slider this many pixels to the right | `x: 42` → drag 42px right |
| `click` | Click at these (x, y) pixel coordinates on the image | `clicks: [{x: 120, y: 45}]` |
| `single` | Boolean per tile — select the `true` ones | `hasObject: [true, false, true]` |

## Step 5: Present the Answer

Explain the solution clearly to the user:
- For **grid**: "Click tiles at positions 1, 3, 5 (0-indexed, counting left-to-right, top-to-bottom)"
- For **text**: "Enter the text: abc123"
- For **slide**: "Drag the slider 42 pixels to the right"
- For **click**: "Click at coordinates (120, 45) and (200, 80) on the image"

## Fallback: Python SDK

If the MCP server is not available, fall back to the Python SDK:

```bash
pip install captchasonic
```

```python
from captchasonic import SonicClient
from pathlib import Path

client = SonicClient(api_key="sonic_xxxx")
result = client.create_task({
    "type": "PopularCaptchaImage",
    "question": "Select all traffic lights",
    "images": [open("captcha.png", "rb").read()],
})

ts = result.typed_solution
if ts.HasField("grid"):
    print("Grid objects:", list(ts.grid.objects))
elif ts.HasField("text"):
    print("Text:", list(ts.text.texts))
elif ts.HasField("slide"):
    print("Slide X:", ts.slide.x, "px")
elif ts.HasField("click"):
    print("Clicks:", [(c.x, c.y) for g in ts.click.groups for c in g.coords])
client.close()
```
