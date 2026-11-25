# Interactive Widgets

## Base styles (start every interactive widget with these)
```html
<style>
.w-card{background:var(--color-surface);border:1px solid var(--color-border);border-radius:12px;padding:24px}
.w-label{display:flex;justify-content:space-between;font-size:12px;line-height:16px;font-weight:500;color:var(--color-text-muted);margin-bottom:8px}
.w-label strong{color:var(--color-text);font-weight:500;font-family:var(--font-mono)}
.w-range{width:100%;accent-color:var(--color-accent);cursor:pointer}
.w-input{width:100%;padding:8px 12px;background:var(--color-bg);border:1px solid var(--color-border);border-radius:8px;color:var(--color-text);font:inherit;font-size:14px;line-height:20px;transition:border-color 300ms cubic-bezier(0.22,1,0.36,1)}
.w-input:focus{outline:2px solid var(--color-accent);outline-offset:2px}
.w-btn{display:inline-flex;align-items:center;gap:8px;padding:8px 12px;border:none;border-radius:8px;background:var(--color-text);color:var(--color-bg);font:inherit;font-size:14px;line-height:20px;font-weight:600;cursor:pointer;transition:opacity 300ms cubic-bezier(0.22,1,0.36,1),transform 300ms cubic-bezier(0.22,1,0.36,1)}
.w-btn:hover{opacity:.86}.w-btn:active{transform:scale(.98)}
.w-btn:focus-visible{outline:2px solid var(--color-accent);outline-offset:2px}
.w-btn.secondary{background:var(--color-bg);color:var(--color-text);border:1px solid var(--color-border)}
.w-btn.secondary:hover{opacity:1;background:var(--color-surface-elevated)}
.w-btn.accent{background:var(--color-accent);color:var(--color-on-accent)}
</style>
```

Primary buttons are inverted (text color on background color), like Vercel. Use `.accent` only for one highlight action.

## Sliders and Controls
```html
<div style="margin-bottom:24px">
  <label class="w-label" for="rate">Annual rate <strong id="rv">7%</strong></label>
  <input id="rate" class="w-range" type="range" min="1" max="20" step="0.5" value="7"
         oninput="document.getElementById('rv').textContent=this.value+'%'; recalc()">
</div>
```

Show the live value in the label and update it with `oninput`.

## Metric / Result Card
```html
<div class="w-card" style="margin-bottom:16px">
  <div style="font-size:12px;line-height:16px;font-weight:500;color:var(--color-text-muted);margin-bottom:8px">Final amount</div>
  <div style="font-size:30px;line-height:36px;font-weight:600;letter-spacing:-0.02em;color:var(--color-text);font-variant-numeric:tabular-nums" id="result">$19,671.51</div>
  <div style="font-size:12px;line-height:16px;color:var(--color-text-muted);margin-top:4px">
    Interest earned <span id="interest" style="color:var(--color-success)">$9,671.51</span>
  </div>
</div>
```

## Input Field
```html
<input type="text" class="w-input" placeholder="Enter a value">
```

## Button
```html
<button class="w-btn">Calculate</button>
<button class="w-btn secondary">Reset</button>
```

## Script Pattern for Calculators
Keep all state in top level `let` variables and use `oninput` on each control.

```html
<script>
let principal = 10000, rate = 7, years = 10;

function recalc() {
  const amount = principal * Math.pow(1 + rate / 100, years);
  document.getElementById('result').textContent =
    amount.toLocaleString(undefined, { style: 'currency', currency: 'USD' });
}

recalc();
</script>
```

## Timer Pattern
```html
<script>
let seconds = 0;
const display = document.getElementById('time');
const timer = setInterval(() => {
  seconds++;
  display.textContent = String(Math.floor(seconds/60)).padStart(2,'0')
    + ':' + String(seconds%60).padStart(2,'0');
}, 1000);
</script>
```
Display timers with `font-family:var(--font-mono)` or `font-variant-numeric:tabular-nums` so digits do not jump.

## Grid Layout for Controls
```html
<div style="display:grid;grid-template-columns:repeat(auto-fit,minmax(220px,1fr));gap:16px;margin-bottom:24px">
  <!-- controls -->
</div>
```
