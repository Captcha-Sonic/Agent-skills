# Example: OCR Text Recognition

## Scenario

A website shows a distorted text image that you need to type into a field to prove you're human.

## Step 1: Capture the Image

Save or screenshot the CAPTCHA image. For example: `captcha_text.png`

The image looks something like:
```
╔═══════════════════╗
║  aB3x9Kp          ║
╚═══════════════════╝
```

## Step 2: Base64-Encode the Image

If you have a file path:
```bash
base64 -i captcha_text.png
```

Or if the user provides a URL, use `image_url` directly.

## Step 3: Call the MCP Tool

```
Tool: solve_captcha
Arguments:
  type: "ocr"
  question: "What text is in this image?"
  image_base64: "/9j/4AAQSkZJRgABAQ..."
```

## Step 4: Read the Response

```json
{
  "status": "ready",
  "taskId": "task-ocr-456",
  "type": "text",
  "texts": ["aB3x9Kp"]
}
```

## Step 5: Use the Answer

Type `aB3x9Kp` into the CAPTCHA input field on the website.

**Important:** OCR solutions are case-sensitive. Enter the text exactly as returned.

## Advanced OCR Options

When using the Python SDK directly, you can hint at the expected format:

```python
result = client.solve_ocr(
    images=[open("captcha.png", "rb").read()],
    numeric=False,      # True if the captcha is numbers only
    min_length=4,       # Minimum expected characters
    max_length=8,       # Maximum expected characters
)
```

These hints are not yet available via the MCP tool — use the Python SDK for advanced OCR control.

## Cost

A typical OCR solve costs approximately **$0.0005 - $0.001 USD**.
