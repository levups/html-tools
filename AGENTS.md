# HTML Tools - Development Guidelines

## Philosophy

Simple, browser-based utilities. No build step. No frameworks. Just HTML, CSS, and vanilla JavaScript.

## Code Style

### Accessibility (WCAG)
- Always write accessible HTML
- Use semantic elements (`<header>`, `<main>`, `<nav>`, `<button>`, etc.)
- Include proper `aria-` attributes where needed
- Ensure sufficient color contrast (WCAG AA minimum)
- All interactive elements must be keyboard accessible
- Form inputs must have associated labels
- Images need meaningful `alt` text

### No External Services from Big Tech
- **NO Google Fonts** - Use system font stacks instead
- **NO Google CDN, Amazon CloudFront, or similar**
- If external JS dependencies are absolutely needed, use **Bunny CDN** (bunny.net)
- Example: `https://cdn.jsdelivr.net/npm/package@version/file.min.js` → use Bunny equivalent

### Prefer Native/Built-in Solutions
- Always try to implement functionality in vanilla JS first
- Use modern browser APIs (2025+) - they're powerful enough for most tasks
- Only add external dependencies if reimplementing would be unreasonably complex
- Examples of things to implement yourself:
  - JSON parsing/formatting (built-in)
  - Base64 encoding (built-in `btoa`/`atob`)
  - URL encoding (built-in `encodeURIComponent`)
  - Date/time handling (built-in `Intl.DateTimeFormat`)
  - Simple markdown parsing
  - Diff algorithms

### No React/Vue/Angular
- No frontend frameworks
- No JSX, no virtual DOM
- Plain DOM manipulation is fine and fast enough

### Font Stack
Use this system font stack in CSS:
```css
font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif;
```

For monospace:
```css
font-family: ui-monospace, SFMono-Regular, "SF Mono", Menlo, Consolas, "Liberation Mono", monospace;
```

## File Structure

- `index.html` - Tool listing with filter
- `common.css` - Shared styles
- `[tool-name].html` - Individual tools (self-contained)
- `AGENTS.md` - This file (coding guidelines)
- `CLAUDE.md` - Points AI assistants to this file

## Tool Patterns

### Bidirectional Converters
For encoder/decoder tools, make both sides editable with real-time sync:
- Text ↔ Base64
- Text ↔ URL encoded
- Text ↔ HTML entities

Use an `updating` flag to prevent infinite loops:
```javascript
let updating = false;

function updateA() {
    if (updating) return;
    updating = true;
    // ... update B from A
    updating = false;
}
```

### Shared CSS
All tools should `<link rel="stylesheet" href="common.css">` and only add page-specific styles inline.

### Index Integration
When adding or updating a tool:
1. Create/update the tool HTML file
2. Add/update a card in `index.html` with appropriate `data-keywords`
3. **Keep tools sorted alphabetically** in `index.html`
4. **Always update `README.md`** to reflect the new/updated tool

## External Dependencies (if absolutely needed)

Use jsDelivr (Bunny CDN backed):
```html
<script src="https://cdn.jsdelivr.net/npm/[package]@[version]/[file]"></script>
```

Current dependencies:
- `js-yaml@4.1.0` - YAML parsing (complex grammar)
- `marked@15.0.4` - Markdown parsing (battle-tested since 2011)
