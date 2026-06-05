# Solution Types

The `solve_captcha` MCP tool returns a JSON object with a `type` field indicating how to interpret the solution.

## Grid Solution

**Used by:** PopularCaptcha, reCAPTCHA v2, AWS WAF, Geetest Nine-Grid

```json
{
  "type": "grid",
  "objects": [1, 3, 5]
}
```

**Interpretation:** The numbers are **0-indexed tile positions**, counted left-to-right, top-to-bottom.

```
For a 3×3 grid:
┌───┬───┬───┐
│ 0 │ 1 │ 2 │
├───┼───┼───┤
│ 3 │ 4 │ 5 │
├───┼───┼───┤
│ 6 │ 7 │ 8 │
└───┴───┴───┘

objects: [1, 3, 5] → click tiles at positions 1, 3, 5
```

## Text Solution

**Used by:** OCR, Audio transcription

```json
{
  "type": "text",
  "texts": ["abc123"]
}
```

**Interpretation:** Type the text `abc123` into the captcha input field. The array usually has one element.

## Slide Solution

**Used by:** Geetest Slide, Binance, TikTok Whirl, Generic Slider

```json
{
  "type": "slide",
  "x": 42
}
```

**Interpretation:** Drag the slider **42 pixels to the right** from its starting position.

## Click Solution

**Used by:** Geetest Click, TikTok

```json
{
  "type": "click",
  "groups": [
    {
      "clicks": [
        { "x": 120, "y": 45 },
        { "x": 200, "y": 80 }
      ]
    }
  ]
}
```

**Interpretation:** Click at pixel coordinates `(120, 45)` and `(200, 80)` on the captcha image, in order.

## Single Solution

**Used by:** reCAPTCHA single-image verification

```json
{
  "type": "single",
  "hasObject": [true, false, true]
}
```

**Interpretation:** One boolean per tile. Select the tiles where the value is `true`.

## Token Solution (Async)

**Used by:** Turnstile, reCAPTCHA v2/v3 Token, Cloudflare Challenge

Token solutions are returned by the standard SDK polling mechanism. The MCP tool returns:

```json
{
  "status": "ready",
  "taskId": "abc-123",
  "type": "text",
  "texts": ["0.token_string_here..."]
}
```

**Interpretation:** Inject this token string as the captcha response in the target website.
