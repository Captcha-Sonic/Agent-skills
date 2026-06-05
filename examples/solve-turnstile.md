# Example: Solving Cloudflare Turnstile

## Scenario

A website at `https://example.com/login` is protected by Cloudflare Turnstile. You need to get a valid token to submit the form.

## Key Difference

Turnstile is a **token-based** captcha — there's no image to solve. Instead, the API:
1. Visits the website in a headless browser
2. Waits for Turnstile to generate a token
3. Returns the token string

This is **asynchronous** and can take 10–60 seconds.

## Step 1: Get the Required Info

You need:
- `website_url` — the page with the Turnstile widget
- `website_key` — the Turnstile site key (found in the page's HTML)

The site key is in the HTML source:
```html
<div class="cf-turnstile" data-sitekey="0x4AAAAAAABkMYinukE8nzYS"></div>
```

## Step 2: Call the MCP Tool

```
Tool: solve_captcha
Arguments:
  type: "turnstile"
  question: "Cloudflare Turnstile"
  website_url: "https://example.com/login"
```

## Step 3: Wait for the Response

The API will poll internally. After 10–60 seconds:

```json
{
  "status": "ready",
  "taskId": "task-xyz-789",
  "type": "text",
  "texts": ["0.erY5mVn3J7vKQB4EJvfM5sjDFjAiQI..."]
}
```

## Step 4: Use the Token

The token in `texts[0]` is injected into the page as the Turnstile response:

```javascript
// In the browser console or via automation:
document.querySelector('[name="cf-turnstile-response"]').value = "0.erY5mVn3J7vKQB4EJvfM5sjDFjAiQI...";
```

Or submit it as a form field in your HTTP request:
```
POST /login
Content-Type: application/x-www-form-urlencoded

cf-turnstile-response=0.erY5mVn3J7vKQB4EJvfM5sjDFjAiQI...&username=user&password=pass
```

## Cost

A typical Turnstile solve costs approximately **$0.002 - $0.005 USD**.
