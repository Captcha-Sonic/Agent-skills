# Example: Solving a reCAPTCHA v2 Grid

## Scenario

A website shows a 3×3 image grid with the question: **"Select all images with traffic lights"**

## Step 1: Capture the Image

The captcha image can be provided as:
- A **file path**: `/Users/you/Downloads/recaptcha.png`
- A **URL**: `https://example.com/captcha-image.png`
- **Base64-encoded** bytes

## Step 2: Call the MCP Tool

```
Tool: solve_captcha
Arguments:
  type: "PopularCaptchaImage"
  question: "Select all images with traffic lights"
  image_base64: "/9j/4AAQSkZJRgABAQ..."  (or use image_url instead)
```

## Step 3: Read the Response

```json
{
  "status": "ready",
  "taskId": "task-abc-123",
  "type": "grid",
  "objects": [0, 3, 6]
}
```

## Step 4: Interpret

The grid is laid out left-to-right, top-to-bottom:

```
┌───┬───┬───┐
│ 0 │ 1 │ 2 │  ← objects [0] = top-left ✅
├───┼───┼───┤
│ 3 │ 4 │ 5 │  ← objects [3] = middle-left ✅
├───┼───┼───┤
│ 6 │ 7 │ 8 │  ← objects [6] = bottom-left ✅
└───┴───┴───┘
```

**Answer:** Click tiles at positions 0, 3, and 6 (the entire left column).

## Cost

A typical reCAPTCHA v2 grid solve costs approximately **$0.001 - $0.002 USD**.
