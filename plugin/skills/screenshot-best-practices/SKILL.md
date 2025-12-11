---
name: Chrome Screenshot Best Practices
description: This skill should be used when taking screenshots with Chrome DevTools MCP, capturing browser content, or when screenshots are too large. Provides guidance on optimal viewport sizing for Retina/HiDPI displays.
version: 1.0.0
---

# Chrome Screenshot Best Practices

## The Retina/HiDPI Display Problem

On Retina/HiDPI displays (common on Mac and many modern monitors), Chrome DevTools captures screenshots at the device pixel ratio (usually 2x). This means:

- A 2560x1664 logical viewport produces a 5120x3328 pixel screenshot
- These oversized images waste context tokens
- They may exceed size limits or be truncated by Claude

## The Solution: Resize Before Screenshot

**Always resize the page before taking a screenshot:**

```
1. Call mcp__chrome-devtools__resize_page with width: 1280, height: 800
2. Then call mcp__chrome-devtools__take_screenshot
```

This produces screenshots at 2560x1600 pixels (2x of 1280x800), which is manageable.

## Recommended Viewport Sizes

| Use Case | Width | Height | Result (2x) |
|----------|-------|--------|-------------|
| Standard desktop | 1280 | 800 | 2560x1600 |
| Smaller desktop | 1024 | 768 | 2048x1536 |
| Mobile (portrait) | 375 | 812 | 750x1624 |
| Mobile (landscape) | 812 | 375 | 1624x750 |
| Tablet (portrait) | 768 | 1024 | 1536x2048 |
| Tablet (landscape) | 1024 | 768 | 2048x1536 |

## Full Page Screenshots

For full-page screenshots (`fullPage: true`), the height doesn't matter as much since the entire scrollable content is captured. Keep the width reasonable:

```javascript
// Resize first
mcp__chrome-devtools__resize_page({ width: 1280, height: 800 })

// Then capture full page
mcp__chrome-devtools__take_screenshot({ fullPage: true })
```

## Element Screenshots

When capturing a specific element by `uid`, the viewport size still affects the element's rendering. Resize to ensure consistent results:

```javascript
// Resize first
mcp__chrome-devtools__resize_page({ width: 1280, height: 800 })

// Then capture element
mcp__chrome-devtools__take_screenshot({ uid: "element-uid" })
```

## Quick Reference

Before ANY screenshot with Chrome DevTools MCP:

1. `mcp__chrome-devtools__resize_page({ width: 1280, height: 800 })`
2. `mcp__chrome-devtools__take_screenshot()`

This ensures consistent, reasonably-sized screenshots regardless of the host display's pixel density.

## Why This Matters

- **Token efficiency**: Smaller images use fewer tokens in Claude's context
- **Reliability**: Prevents truncation or rejection of oversized images
- **Consistency**: Same viewport size = reproducible screenshots across different machines
- **Performance**: Smaller images process faster
