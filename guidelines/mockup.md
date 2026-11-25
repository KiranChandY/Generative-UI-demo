# UI Mockups and Dashboards

## Base styles
```html
<style>
.m-card{background:var(--color-surface);border:1px solid var(--color-border);border-radius:12px;padding:16px}
.m-label{font-size:12px;line-height:16px;font-weight:500;color:var(--color-text-muted)}
.m-value{font-size:30px;line-height:36px;font-weight:600;letter-spacing:-0.02em;color:var(--color-text);font-variant-numeric:tabular-nums;margin-top:8px}
.m-delta{font-size:12px;line-height:16px;font-weight:500;margin-top:4px}
.m-title{font-size:16px;line-height:24px;font-weight:500;color:var(--color-text)}
.m-badge{display:inline-flex;align-items:center;gap:4px;padding:2px 8px;border-radius:9999px;font-size:12px;line-height:16px;font-weight:500}
</style>
```

## Stat Grid (metric cards)
```html
<div style="display:grid;grid-template-columns:repeat(auto-fill,minmax(180px,1fr));gap:12px;margin-bottom:16px">
  <div class="m-card">
    <div class="m-label">Revenue</div>
    <div class="m-value">$48,213</div>
    <div class="m-delta" style="color:var(--color-success)">↑ 12.4% vs last month</div>
  </div>
</div>
```

Use believable numbers with odd final digits or decimals (48,213 not 48,000).

## Badge / Tag
```html
<span class="m-badge" style="background:color-mix(in srgb,var(--color-accent) 14%,transparent);color:var(--color-accent-light)">Active</span>
```

Status variants:
- Success: `background:color-mix(in srgb,var(--color-success) 14%,transparent);color:var(--color-success)`
- Warning: `background:color-mix(in srgb,var(--color-warning) 14%,transparent);color:var(--color-warning)`
- Danger: `background:color-mix(in srgb,var(--color-danger) 14%,transparent);color:var(--color-danger)`
- Neutral: `background:var(--color-surface-elevated);color:var(--color-text-muted)`

## Progress Bar
```html
<div style="background:var(--color-surface-elevated);border-radius:9999px;height:6px;margin:8px 0;overflow:hidden">
  <div style="background:var(--color-accent);border-radius:9999px;height:6px;width:65%;transform-origin:left"></div>
</div>
```

## Data Table
```html
<div class="m-card" style="padding:0;overflow:hidden">
<table style="width:100%;border-collapse:collapse;font-size:14px;line-height:20px">
  <thead>
    <tr style="border-bottom:1px solid var(--color-border)">
      <th class="m-label" style="padding:12px 16px;text-align:left">Name</th>
      <th class="m-label" style="padding:12px 16px;text-align:right">Value</th>
    </tr>
  </thead>
  <tbody>
    <tr style="border-bottom:1px solid var(--color-border)">
      <td style="padding:12px 16px;color:var(--color-text)">Row name</td>
      <td style="padding:12px 16px;text-align:right;color:var(--color-text-muted);font-variant-numeric:tabular-nums">Value</td>
    </tr>
  </tbody>
</table>
</div>
```

## Section Card
```html
<div class="m-card" style="padding:24px;margin-bottom:16px">
  <div class="m-title" style="margin-bottom:16px">Section title</div>
  <!-- content -->
</div>
```

## Icon + Label Row
```html
<div style="display:flex;align-items:center;gap:12px;padding:12px 0;border-bottom:1px solid var(--color-border)">
  <div style="width:32px;height:32px;flex-shrink:0;display:grid;place-items:center;border-radius:8px;
              background:var(--color-surface-elevated);color:var(--color-text-muted);font-size:16px">⚡</div>
  <div style="flex:1;min-width:0">
    <div style="font-size:14px;line-height:20px;font-weight:500;color:var(--color-text)">Feature name</div>
    <div style="font-size:12px;line-height:16px;color:var(--color-text-muted)">Description</div>
  </div>
  <div style="font-size:14px;line-height:20px;font-weight:500;color:var(--color-text)">Value</div>
</div>
```
