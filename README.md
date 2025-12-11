# Claude Chrome MCP Screenshot Fixer

A Claude Code plugin that automatically ensures optimal viewport sizing before taking Chrome DevTools MCP screenshots, preventing oversized images from Retina/HiDPI displays.

## The Problem

On Retina/HiDPI displays, Chrome DevTools MCP captures screenshots at the device pixel ratio (usually 2x). A typical MacBook display at 2560x1664 produces **5120x3328 pixel screenshots** - far too large for efficient use in Claude's context window.

### Error Messages This Plugin Fixes

If you're seeing these errors, this plugin is for you:

```
API Error: 400 {"type":"error","error":{"type":"invalid_request_error","message":"messages.15.content.2.image.source.base64.data: At least one of the image dimensions exceed max allowed size: 8000 pixels"}}
```

```
Image too large: screenshot dimensions exceed maximum allowed size
```

## The Solution

This plugin intercepts `mcp__chrome-devtools__take_screenshot` calls and reminds Claude to first resize the viewport to a standard size (1280x800), producing manageable 2560x1600 screenshots.

## Installation

### Option 1: Install from GitHub (Recommended)

```bash
claude plugins install github:jrlprost/claude-chrome-mcp-screenshot-fixer
```

### Option 2: Install from Local Directory

```bash
claude plugins install /path/to/claude-chrome-mcp-screenshot-fixer/plugin
```

### Option 3: Manual Installation

1. Clone this repository
2. Copy the `plugin` folder contents to your Claude plugins directory:
   - macOS: `~/.claude/plugins/local/claude-chrome-mcp-screenshot-fixer/`
3. Restart Claude Code

## How It Works

### PreToolUse Hook

The plugin registers a `PreToolUse` hook that triggers before any `mcp__chrome-devtools__take_screenshot` call. It prompts Claude to:

1. First call `mcp__chrome-devtools__resize_page` with `width: 1280, height: 800`
2. Then proceed with the screenshot

### Skill Reference

The plugin also includes a skill with best practices for Chrome screenshots, including:
- Recommended viewport sizes for different use cases
- Full-page screenshot guidance
- Element screenshot tips

## Recommended Viewport Sizes

| Use Case | Width | Height | Result (2x Retina) |
|----------|-------|--------|-------------------|
| Standard desktop | 1280 | 800 | 2560x1600 |
| Smaller desktop | 1024 | 768 | 2048x1536 |
| Mobile (portrait) | 375 | 812 | 750x1624 |
| Tablet | 768 | 1024 | 1536x2048 |

## Requirements

- [Claude Code CLI](https://claude.ai/code)
- [Chrome DevTools MCP server](https://github.com/anthropics/anthropic-quickstarts/tree/main/mcp-servers/chrome-devtools) configured

## License

MIT

## Contributing

Contributions welcome! Please open an issue or PR.

## Keywords

claude-code, claude-code-plugin, chrome-devtools, mcp, screenshot, retina, hidpi
