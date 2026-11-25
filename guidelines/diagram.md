# SVG Diagrams

SVG presentation attributes cannot read CSS variables, so **set colors with the `style` attribute** (`style="fill:var(--color-surface)"`), never with `fill="#..."`. This keeps diagrams correct in both themes.

## SVG Shell
```html
<style>
.d-node{fill:var(--color-surface);stroke:var(--color-border);stroke-width:1}
.d-hi{fill:color-mix(in srgb,var(--color-accent) 14%,transparent);stroke:var(--color-accent);stroke-width:1.5}
.d-ok{fill:color-mix(in srgb,var(--color-success) 14%,transparent);stroke:var(--color-success)}
.d-warn{fill:color-mix(in srgb,var(--color-warning) 14%,transparent);stroke:var(--color-warning)}
.d-bad{fill:color-mix(in srgb,var(--color-danger) 14%,transparent);stroke:var(--color-danger)}
.d-label{fill:var(--color-text);font-size:13px;font-weight:500}
.d-hi-label{fill:var(--color-accent-light);font-size:13px;font-weight:500}
.d-sub{fill:var(--color-text-muted);font-size:12px}
.d-edge{stroke:var(--color-text-muted);stroke-width:1.5;fill:none}
.d-arrow{fill:var(--color-text-muted)}
</style>
<svg viewBox="0 0 800 400" xmlns="http://www.w3.org/2000/svg"
     style="width:100%;height:auto;display:block;font-family:inherit">
  <defs>
    <marker id="arrow" viewBox="0 0 10 10" refX="9" refY="5"
            markerWidth="6" markerHeight="6" orient="auto">
      <path d="M0,0 L10,5 L0,10 Z" class="d-arrow"/>
    </marker>
  </defs>
  <!-- nodes and edges -->
</svg>
```

Put the `<style>` block first and `<defs>` at the top of the SVG so classes and markers exist during streaming.

## Node Types

**Default box:**
```html
<rect x="50" y="50" width="140" height="48" rx="8" class="d-node"/>
<text x="120" y="79" text-anchor="middle" class="d-label">Node label</text>
```

**Highlighted box:**
```html
<rect x="50" y="50" width="140" height="48" rx="8" class="d-hi"/>
<text x="120" y="79" text-anchor="middle" class="d-hi-label">Highlighted</text>
```

**Status boxes:** use `d-ok`, `d-warn` or `d-bad` on the `<rect>`.

**Pill:** set `rx` to half the height.

## Edges and Arrows

**Straight arrow:**
```html
<line x1="190" y1="74" x2="270" y2="74" class="d-edge" marker-end="url(#arrow)"/>
```

**Labeled edge:**
```html
<line x1="190" y1="74" x2="270" y2="74" class="d-edge" marker-end="url(#arrow)"/>
<text x="230" y="64" text-anchor="middle" class="d-sub">calls</text>
```

**Curved path (loopbacks or crossing lines):**
```html
<path d="M 190 74 C 230 30, 270 30, 310 74" class="d-edge" marker-end="url(#arrow)"/>
```

## Layout Tips
- Horizontal flow: left to right, 160 to 180px between node centers
- Vertical flow: top to bottom, 80 to 100px between rows
- Keep the viewBox proportional to content: 800×400 for wide, 600×500 for tall
- At most 2 highlight colors per diagram
- Every `<text>` gets a class with an explicit fill

## Section Headers in Diagrams
```html
<text x="400" y="24" text-anchor="middle" class="d-sub" style="font-weight:500">Section title</text>
```
