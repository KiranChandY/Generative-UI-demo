# Core Design System

The host app has a **dark theme (default) and a light theme**. Widgets must look right in both, so every color comes from a CSS variable. Never hardcode a hex, rgb or named color.

## HTML Structure
- HTML fragments only. No DOCTYPE, `<html>`, `<head>` or `<body>`
- Streaming order: `<style>` → content HTML → `<script>` (critical for smooth streaming)
- No comments (`<!-- -->` or `/* */`). They waste tokens during streaming
- No `display:none` sections, tabs or carousels during streaming. Stack content vertically
- Scripts run after streaming completes, so CDN globals are safe in a trailing `<script>`

## Color tokens (mandatory)
| Variable | Purpose |
|---|---|
| `--color-bg` | Page background (the host paints it, do not repaint the root) |
| `--color-surface` | Card / panel background |
| `--color-surface-elevated` | Raised element, hover fill, track of a progress bar |
| `--color-text` | Primary text |
| `--color-text-muted` | Secondary text, labels, axis ticks |
| `--color-accent` | Brand blue: primary buttons, active states, key highlight |
| `--color-accent-light` | Accent for text and lines on a dark or tinted area |
| `--color-on-accent` | Text placed on an accent background |
| `--color-border` | Hairline borders and dividers |
| `--color-success` / `--color-warning` / `--color-danger` | Status colors |
| `--color-chart-1` … `--color-chart-5` | Categorical palette for charts and diagram series |

**Tints:** for soft backgrounds use `color-mix(in srgb, var(--color-accent) 12%, transparent)`. The same works for success, warning and danger.

## Typography
The host sets the font (Geist). Inherit it, never set `font-family` except `var(--font-mono)` for code or tabular numbers.

| Use | Size / line height | Weight |
|---|---|---|
| Widget title (h1) | 24px / 32px | 600 |
| Section title (h2) | 20px / 28px | 600 |
| Card title (h3) | 16px / 24px | 500 |
| Big metric number | 30px / 36px | 600 |
| Body | 14px / 20px | 400 |
| Label, caption | 12px / 16px | 500 |

- Never below 12px. Never weight above 600. Never italic
- Headings use `letter-spacing:-0.02em` and `text-wrap:balance`
- Sentence case everywhere. Labels never use uppercase transforms

## Spacing
Use only: 2, 4, 8, 12, 16, 24, 32, 40, 48, 64px. Card padding 16 or 24. Gaps between cards 12 or 16.

## Radius
Use only: 4, 6, 8, 12, 16px or 9999px for pills. Cards 12px. Inputs and buttons 8px.
When a shape sits inside another with a gap under 32px: inner radius = outer radius minus the gap.

## Design Rules
- **Flat design only.** No gradients, box shadows, blur, text shadow or glow
- Borders go all the way around an element, never on one side of a card (table row dividers are fine)
- No outer background color on the root container
- Motion: `transition: <property> 300ms cubic-bezier(0.22,1,0.36,1)`. Animate only transform and opacity
- Every button has hover and active states; inputs show `outline:2px solid var(--color-accent)` on focus
- Copy: plain and specific, no exclamation marks, no filler words
- CDN allowed: `cdnjs.cloudflare.com`, `cdn.jsdelivr.net`, `unpkg.com`, `esm.sh` only

## Reading tokens in JavaScript
Canvas libraries cannot read CSS variables directly, so resolve them:
```js
const css = n => getComputedStyle(document.documentElement).getPropertyValue(n).trim();
css('--color-text-muted')  // "#A1A1A1" in dark, "#666666" in light
```
The host runs the widget again when the user switches theme, so resolving once on load is enough.

## Two Way Communication
`window.sendToAgent(data)` sends JSON back to the chat as a user message.

```html
<button onclick="sendToAgent({action:'filter', value:'weekly'})">Weekly</button>
<select onchange="sendToAgent({selected: this.value})">...</select>
```

Use it for filter selections, form submissions, quiz answers and calculator results to analyze.
