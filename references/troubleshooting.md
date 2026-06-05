# Troubleshooting

## Connection Issues

### "health_check failed" or "MCP server not responding"

**Cause:** The MCP server process isn't running or can't connect to the CaptchaSonic API.

**Fixes:**
1. Check Node.js is installed: `node --version` (need v18+)
2. Try running the server manually: `npx -y @captchasonic/mcp-server`
3. Check internet connectivity: `curl -s https://api.captchasonic.com/rpc/captchasonic.v1.SonicService/HealthCheck -X POST -d '{}'`
4. Re-add the MCP server: `claude mcp remove sonic && claude mcp add sonic --env SONIC_API_KEY=sonic_xxxx -- npx -y @captchasonic/mcp-server`

### "Cannot find module @captchasonic/mcp-server"

**Cause:** npm package not accessible.

**Fixes:**
1. Clear npm cache: `npm cache clean --force`
2. Install globally instead: `npm install -g @captchasonic/mcp-server`
3. Use `sonic-mcp` directly: `claude mcp add sonic --env SONIC_API_KEY=sonic_xxxx -- sonic-mcp`

## API Key Issues

### Error: "SONIC_API_KEY is not set"

**Cause:** The environment variable wasn't passed to the MCP server.

**Fix:** Re-add with the key:
```bash
claude mcp remove sonic
claude mcp add sonic --env SONIC_API_KEY=sonic_xxxx -- npx -y @captchasonic/mcp-server
```

### Error: "errorId: 1" (Invalid API Key)

**Cause:** The API key is wrong or expired.

**Fix:** Verify your key at [captchasonic.com](https://captchasonic.com). Keys must start with `sonic_`.

### Error: "errorId: 2" (Insufficient Balance)

**Cause:** Your account has no credits left.

**Fix:** Top up at [captchasonic.com](https://captchasonic.com).

## Solving Issues

### Error: "errorId: 12" (Task Unsolvable)

**Cause:** The captcha image was too blurry, too small, or unrecognizable.

**Fixes:**
1. Use a higher-resolution image
2. Make sure the image is a standard CAPTCHA (PNG/JPEG)
3. Verify you're using the correct `type` parameter

### Timeout during token-based solving

**Cause:** Token-based captchas (Turnstile, reCAPTCHA token) can take up to 120 seconds.

**Fixes:**
1. Wait longer — some captchas take 30-60 seconds
2. Check if `website_url` is correct
3. For Cloudflare Challenge, a proxy may be required

### Wrong solution format

**Cause:** Using the wrong `type` parameter.

**Fix:** Refer to `references/captcha-types.md` for the correct type string.

## Error ID Reference

| Error ID | Name | Description |
|---|---|---|
| 0 | Success | No error |
| 1 | InvalidApiKey | API key is wrong or expired |
| 2 | InsufficientBalance | No credits remaining |
| 3 | DailyLimitExceeded | Daily usage limit reached |
| 4 | MinuteLimitExceeded | Per-minute rate limit hit |
| 5 | QuotaExceeded | Plan quota exceeded |
| 6 | PlanExpired | Subscription has expired |
| 12 | TaskUnsolvable | Image too blurry or unrecognizable |
| 17-19 | RateLimit | Too many requests, slow down |
