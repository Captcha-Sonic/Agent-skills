# Supported CAPTCHA Types

## Image-Based (Synchronous)

These CAPTCHAs are solved instantly — the API returns the solution in the same response.

| Type | `type` Parameter | `question` Example | Solution Format |
|---|---|---|---|
| **PopularCaptcha** | `PopularCaptchaImage` | "Select all traffic lights" | `grid` → tile indices |
| **reCAPTCHA v2 Grid** | `PopularCaptchaImage` | "Select all crosswalks" | `grid` → tile indices |
| **Geetest Slide** | `geetest_slide` | (not needed) | `slide` → pixel offset |
| **Geetest Click** | `geetest_click` | "Click the objects in order" | `click` → coordinates |
| **Geetest Nine-Grid** | `geetest_nine` | "Select matching images" | `grid` → tile indices |
| **OCR / Image-to-Text** | `ocr` | "What text is in this image?" | `text` → recognized text |
| **AWS WAF** | `aws` | "Select all cars" | `grid` → tile indices |
| **TikTok** | `tiktok` | "tiktok_whirl" | `slide` → rotation offset |
| **Binance Slide** | `binance` | "binance_slide" | `slide` → pixel offset |
| **Generic Slider** | `slide` | (not needed) | `slide` → pixel offset |

## Token-Based (Asynchronous)

These CAPTCHAs are solved by automated browsers — the API polls until the token is ready.

| Type | `type` Parameter | Required Fields | Solution |
|---|---|---|---|
| **Cloudflare Turnstile** | `turnstile` | `website_url` | Token string |
| **reCAPTCHA v2 Token** | `recaptcha_v2` | `website_url` | Token string |
| **reCAPTCHA v3 Token** | `recaptcha_v3` | `website_url` | Token string |
| **Cloudflare Challenge** | `cloudflare` | `website_url` | Token string |
| **PopularCaptcha Token** | `popularcaptcha_token` | `website_url` | Token string |

## MCP Tool Parameters

The `solve_captcha` MCP tool accepts:

```
{
  "type": "string",          // Required — captcha type from tables above
  "question": "string",      // Required — challenge text or task description
  "image_base64": "string",  // Image as base64 — for image-based captchas
  "image_url": "string",     // OR public URL of the image
  "website_url": "string"    // For token-based captchas — the page URL
}
```

**Rules:**
- Provide `image_base64` OR `image_url`, not both
- For token-based captchas, `website_url` is required
- For image-based captchas, at least one image source is required
- `question` describes the challenge (e.g., "Select all traffic lights")

## Question Types (PopularCaptcha)

PopularCaptcha supports different challenge modes. Specify via the `question` field:

| Question Type | Description | Example Question |
|---|---|---|
| `objectClassify` | Select all tiles matching a category | "Select all traffic lights" |
| `objectClick` | Click specific objects in the image | "Click on all fire hydrants" |
| `objectDrag` | Drag objects to correct positions | "Drag the puzzle piece" |
