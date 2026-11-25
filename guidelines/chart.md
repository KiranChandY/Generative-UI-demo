# Charts with Chart.js

## Loading
```html
<div style="background:var(--color-surface);border:1px solid var(--color-border);border-radius:12px;padding:16px">
  <canvas id="chart" style="width:100%;max-height:360px"></canvas>
</div>
<script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.0/dist/chart.umd.min.js"></script>
<script>
const css = n => getComputedStyle(document.documentElement).getPropertyValue(n).trim();
const C = {
  text: css('--color-text'), muted: css('--color-text-muted'), border: css('--color-border'),
  s1: css('--color-chart-1'), s2: css('--color-chart-2'), s3: css('--color-chart-3'),
  s4: css('--color-chart-4'), s5: css('--color-chart-5')
};
Chart.defaults.font.family = getComputedStyle(document.body).fontFamily;
Chart.defaults.font.size = 12;
Chart.defaults.color = C.muted;
new Chart(document.getElementById('chart'), { /* config */ });
</script>
```

Always load Chart.js with `<script src>` first, then your own `<script>` block. Never hardcode colors: resolve them from CSS variables as above so the chart works in both themes.

## Base Config Template
```javascript
{
  type: 'bar', // bar | line | pie | doughnut | radar | scatter
  data: {
    labels: ['Jan', 'Feb', 'Mar', 'Apr', 'May'],
    datasets: [{
      label: 'Revenue',
      data: [12400, 15130, 11870, 18260, 22410],
      backgroundColor: C.s1,
      borderRadius: 6,
      maxBarThickness: 40
    }]
  },
  options: {
    responsive: true,
    plugins: {
      legend: { labels: { color: C.muted, boxWidth: 8, boxHeight: 8, usePointStyle: true } },
      tooltip: { backgroundColor: C.text, titleColor: css('--color-bg'), bodyColor: css('--color-bg'), padding: 8, cornerRadius: 8 }
    },
    scales: {
      x: { ticks: { color: C.muted }, grid: { display: false }, border: { color: C.border } },
      y: { ticks: { color: C.muted }, grid: { color: C.border }, border: { display: false } }
    }
  }
}
```

## Palette
Use `C.s1` to `C.s5` in that order. For a translucent fill append an alpha to the hex: `C.s1 + '26'` (15%), `C.s1 + 'A6'` (65%).

## Line Chart
```javascript
{
  type: 'line',
  data: {
    labels: [...],
    datasets: [{
      label: 'Series',
      data: [...],
      borderColor: C.s1,
      backgroundColor: C.s1 + '1A',
      borderWidth: 2,
      tension: 0.35,
      fill: true,
      pointRadius: 0,
      pointHoverRadius: 4
    }]
  }
}
```

## Multiple datasets
Use 2 to 4 datasets max, each with its own palette color in order.

## Doughnut / Pie
```javascript
{
  type: 'doughnut',
  data: {
    labels: ['A', 'B', 'C'],
    datasets: [{ data: [31, 46, 23], backgroundColor: [C.s1, C.s2, C.s3], borderWidth: 0 }]
  },
  options: {
    plugins: { legend: { position: 'bottom', labels: { color: C.muted, padding: 16, usePointStyle: true } } },
    cutout: '68%'
  }
}
```
