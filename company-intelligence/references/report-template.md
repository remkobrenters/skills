# Company Intelligence Report Template

All reports are generated as standalone `.html` files using `create_file`, then shared with `present_files`. Never use `show_widget` for reports. No CDN framework dependencies.

---

## Delivery rule

```
create_file → /mnt/user-data/outputs/{company-slug}-intelligence.html
present_files → share with user
```

---

## Styling approach

Use a self-contained `<style>` block. No CDN. No framework. Works offline. No console warnings.

Copy the full CSS block below verbatim at the start of every report. Do not modify it.

---

## CSS block (copy verbatim into every report)

```css
*, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }
body { font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Helvetica, Arial, sans-serif; font-size: 14px; line-height: 1.6; color: #111827; background: #f9fafb; padding: 24px; }
.wrap { max-width: 900px; margin: 0 auto; }
a { color: #2563eb; text-decoration: none; }
a:hover { text-decoration: underline; }

/* Cards */
.card { background: #fff; border: 1px solid #e5e7eb; border-radius: 12px; padding: 20px 24px; margin-bottom: 14px; }
.card-header { border-left: 4px solid #111827; padding-left: 20px; }

/* Header */
.header-inner { display: flex; align-items: flex-start; gap: 20px; }
.logo { width: 56px; height: 56px; border-radius: 8px; background: #111827; color: #fff; display: flex; align-items: center; justify-content: center; font-size: 16px; font-weight: 500; flex-shrink: 0; }
.logo img { width: 100%; height: 100%; object-fit: contain; border-radius: 8px; }
.header-text h1 { font-size: 20px; font-weight: 500; color: #111827; }
.header-text p { font-size: 12px; color: #6b7280; margin-top: 3px; }
.chips { display: flex; flex-wrap: wrap; gap: 6px; margin-top: 10px; }
.chip { font-size: 11px; color: #4b5563; background: #f3f4f6; border: 1px solid #e5e7eb; border-radius: 20px; padding: 2px 10px; }
.chip-warn { background: #fffbeb; border-color: #fde68a; color: #92400e; }

/* Meta bar */
.meta-bar { font-size: 11px; color: #9ca3af; background: #f9fafb; border: 1px solid #e5e7eb; border-radius: 8px; padding: 8px 14px; margin-bottom: 16px; display: flex; flex-wrap: wrap; gap: 16px; }

/* Legend */
.legend { background: #fff; border: 1px solid #e5e7eb; border-radius: 8px; padding: 10px 16px; margin-bottom: 16px; display: flex; flex-wrap: wrap; gap: 16px; align-items: center; }
.legend-label { font-size: 10px; font-weight: 600; color: #9ca3af; text-transform: uppercase; letter-spacing: 0.06em; }
.legend-item { display: flex; align-items: center; gap: 6px; font-size: 11px; color: #4b5563; }

/* Stats grid */
.stats { display: grid; grid-template-columns: repeat(4, 1fr); gap: 10px; margin-bottom: 16px; }
.stat { background: #fff; border: 1px solid #e5e7eb; border-radius: 10px; padding: 14px 12px; text-align: center; }
.stat-value { font-size: 17px; font-weight: 500; color: #111827; }
.stat-label { font-size: 10px; color: #9ca3af; text-transform: uppercase; letter-spacing: 0.05em; margin-top: 3px; }
.stat-source { font-size: 10px; color: #9ca3af; margin-top: 3px; }

/* Section title */
.section-title { font-size: 10px; font-weight: 600; color: #9ca3af; text-transform: uppercase; letter-spacing: 0.07em; margin-bottom: 12px; padding-bottom: 8px; border-bottom: 1px solid #f3f4f6; }

/* Data rows */
.row { display: flex; align-items: flex-start; padding: 6px 0; border-bottom: 1px solid #f9fafb; }
.row:last-child { border-bottom: none; }
.row-label { width: 168px; flex-shrink: 0; font-size: 11px; color: #6b7280; font-weight: 500; padding-top: 2px; }
.row-value { flex: 1; font-size: 13px; color: #1f2937; }
.row-basis { display: block; font-size: 11px; color: #9ca3af; margin-top: 2px; }

/* Badges */
.badge { display: inline-flex; align-items: center; padding: 1px 6px; border-radius: 4px; font-size: 10px; font-weight: 700; text-transform: uppercase; letter-spacing: 0.04em; border: 1px solid; vertical-align: middle; margin-left: 6px; white-space: nowrap; }
.b-l1 { background: #f0fdf4; color: #166534; border-color: #bbf7d0; }
.b-l2 { background: #fefce8; color: #854d0e; border-color: #fde68a; }
.b-l3 { background: #eff6ff; color: #1e40af; border-color: #bfdbfe; }
.b-nf { background: #f9fafb; color: #6b7280; border-color: #e5e7eb; }

/* Tables */
table { width: 100%; border-collapse: collapse; font-size: 13px; }
th { text-align: left; font-size: 10px; font-weight: 600; color: #9ca3af; text-transform: uppercase; letter-spacing: 0.05em; padding: 6px 10px; border-bottom: 1px solid #e5e7eb; background: #f9fafb; }
td { padding: 7px 10px; border-bottom: 1px solid #f3f4f6; color: #374151; vertical-align: top; }
tr:last-child td { border-bottom: none; }
.td-label { color: #6b7280; }
.td-metric { font-weight: 500; color: #111827; }
.td-muted { font-size: 11px; color: #9ca3af; }
.td-result-nf { font-size: 12px; color: #6b7280; }

/* Context flags */
.flags { display: flex; flex-wrap: wrap; gap: 6px; margin-top: 4px; }
.flag { font-size: 11px; color: #374151; background: #f9fafb; border: 1px solid #e5e7eb; border-left: 2px solid #9ca3af; padding: 3px 8px; }
.flag-dark { border-left-color: #374151; }
.flag-signal { border-left-color: #f59e0b; }
.flag-muted { border-left-color: #d1d5db; color: #6b7280; }

/* Competitors */
.comp-list { display: flex; flex-wrap: wrap; gap: 8px; }
.comp { background: #f9fafb; border: 1px solid #e5e7eb; border-left: 2px solid #f59e0b; border-radius: 8px; padding: 5px 12px; }
.comp-adj { border-left-color: #93c5fd; }
.comp-type { font-size: 10px; color: #9ca3af; display: block; margin-bottom: 1px; }
.comp-name { font-size: 12px; color: #1f2937; }

/* Trust signals */
.trust-list { display: flex; flex-wrap: wrap; gap: 7px; }
.trust { font-size: 11px; font-weight: 500; border-radius: 20px; padding: 3px 12px; border: 1px solid; }
.trust-l1 { background: #f0fdf4; border-color: #bbf7d0; color: #166534; }
.trust-l2 { background: #fefce8; border-color: #fde68a; color: #854d0e; }

/* Funding chips */
.fund-list { display: flex; flex-wrap: wrap; gap: 7px; margin-top: 6px; }
.fund-chip { background: #fefce8; border: 1px solid #fde68a; border-radius: 8px; padding: 5px 11px; }
.fund-round { font-size: 11px; font-weight: 600; color: #92400e; display: block; }
.fund-detail { font-size: 11px; color: #b45309; }

/* News */
.news-item { padding: 9px 0; border-bottom: 1px solid #f3f4f6; }
.news-item:last-child { border-bottom: none; }
.news-head { display: flex; align-items: flex-start; flex-wrap: wrap; gap: 6px; }
.news-title { font-size: 13px; font-weight: 500; color: #1f2937; }
.news-meta { font-size: 11px; color: #9ca3af; margin-top: 3px; }

/* L4 Analyst view */
.analyst { border: 1px dashed #d1d5db; border-radius: 12px; padding: 18px 22px; margin-bottom: 14px; background: #f9fafb; }
.analyst-head { display: flex; align-items: center; gap: 10px; margin-bottom: 10px; }
.analyst-tag { background: #fff; border: 1px solid #d1d5db; border-radius: 4px; padding: 2px 8px; font-size: 10px; font-weight: 600; color: #6b7280; text-transform: uppercase; letter-spacing: 0.05em; }
.analyst-disc { font-size: 11px; color: #9ca3af; }
.analyst-body { font-size: 13px; color: #374151; line-height: 1.65; }
.analyst-body p + p { margin-top: 7px; }

/* Dig deeper */
.dig { background: #fff; border: 1px solid #e5e7eb; border-radius: 12px; padding: 16px 20px; }
.dig-title { font-size: 10px; font-weight: 600; color: #9ca3af; text-transform: uppercase; letter-spacing: 0.07em; margin-bottom: 10px; }
.dig-btns { display: flex; flex-wrap: wrap; gap: 7px; }
.dig-btn { font-size: 11px; background: #f9fafb; border: 1px solid #e5e7eb; border-radius: 6px; padding: 5px 11px; color: #374151; cursor: pointer; transition: background 0.1s; }
.dig-btn:hover { background: #f3f4f6; }

@media (max-width: 640px) {
  .stats { grid-template-columns: repeat(2, 1fr); }
  .row { flex-direction: column; gap: 3px; }
  .row-label { width: auto; }
}
```

---

## Page shell

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Intelligence: {COMPANY_NAME}</title>
  <style>
    /* PASTE FULL CSS BLOCK HERE */
  </style>
</head>
<body>
<div class="wrap">
  <!-- content -->
</div>
</body>
</html>
```

---

## Component patterns

### Header

```html
<div class="card card-header" style="margin-bottom: 8px;">
  <div class="header-inner">
    <div class="logo">
      <!-- If logo URL exists: <img src="{URL}" alt="logo"> -->
      <!-- If not: show initials text -->
      {INITIALS}
    </div>
    <div class="header-text">
      <h1>{COMPANY_NAME}</h1>
      <p>{DESCRIPTOR — factual, no adjectives}</p>
      <div class="chips">
        <span class="chip">{FLAG} {COUNTRY}</span>
        <span class="chip">{LEGAL_FORM}</span>
        <span class="chip">{SECTOR}</span>
        <span class="chip">Est. {YEAR}</span>
        <!-- Only add chip-warn if there's a notable status flag (pending acquisition, inactive, etc.) -->
      </div>
    </div>
  </div>
</div>
```

### Meta bar

```html
<div class="meta-bar">
  <span>Generated {DATE}</span>
  <span>Sources: {SOURCE_LIST}</span>
  <span>All figures carry layer badges</span>
</div>
```

### Layer legend

```html
<div class="legend">
  <span class="legend-label">Data layers</span>
  <span class="legend-item"><span class="badge b-l1" style="margin-left:0">VERIFIED</span> Official register / primary filing</span>
  <span class="legend-item"><span class="badge b-l2" style="margin-left:0">ESTIMATE</span> Third-party aggregator</span>
  <span class="legend-item"><span class="badge b-l3" style="margin-left:0">SIGNAL</span> Derived / inferred</span>
  <span class="legend-item"><span class="badge b-nf" style="margin-left:0">NOT FOUND</span> Searched, no result</span>
</div>
```

### Stats row

```html
<div class="stats">
  <div class="stat">
    <div class="stat-value">{VALUE}</div>
    <div class="stat-label">{LABEL}</div>
    <div class="stat-source">{SOURCE} · {YEAR}</div>
  </div>
  <!-- × 4 -->
</div>
```

### Section + data row

```html
<div class="card">
  <div class="section-title">{TITLE}</div>

  <div class="row">
    <span class="row-label">{LABEL}</span>
    <span class="row-value">
      {VALUE}
      <span class="badge b-l1">VERIFIED · KvK</span>
      <span class="row-basis">{BASIS — required only for SIGNAL}</span>
    </span>
  </div>
</div>
```

### Financial table

```html
<div class="card">
  <div class="section-title">Financial signals</div>
  <table>
    <thead>
      <tr>
        <th style="width:140px">Metric</th>
        <th style="width:110px">Value</th>
        <th>Source</th>
        <th style="width:60px">Year</th>
        <th style="width:130px">Layer</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td class="td-label">{METRIC}</td>
        <td class="td-metric">{VALUE}</td>
        <td class="td-muted">{SOURCE}</td>
        <td class="td-muted">{YEAR}</td>
        <td><span class="badge b-l1" style="margin-left:0">VERIFIED</span></td>
      </tr>
    </tbody>
  </table>
</div>
```

### Legal table

```html
<div class="card">
  <div class="section-title">Legal &amp; regulatory</div>
  <table>
    <thead>
      <tr>
        <th>Source searched</th>
        <th style="width:90px">Date</th>
        <th>Result</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td class="td-label">{SOURCE}</td>
        <td class="td-muted">{DATE}</td>
        <td class="td-result-nf">No results returned <span class="badge b-nf" style="margin-left:4px">NOT FOUND</span></td>
      </tr>
    </tbody>
  </table>
</div>
```

### Context flags

```html
<div class="flags">
  <span class="flag flag-dark">Primary focus: {X}</span>
  <span class="flag flag-signal">Signal: {Y}</span>
  <span class="flag flag-muted">Limited public evidence of: {Z}</span>
</div>
```

### Competitors

```html
<div class="comp-list">
  <div class="comp">
    <span class="comp-type">Direct</span>
    <span class="comp-name">{NAME}</span>
  </div>
  <div class="comp comp-adj">
    <span class="comp-type">Adjacent</span>
    <span class="comp-name">{NAME}</span>
  </div>
</div>
```

### Trust signals

```html
<div class="trust-list">
  <span class="trust trust-l1">{CERT} · VERIFIED</span>
  <span class="trust trust-l2">{CERT} · ESTIMATE</span>
</div>
```

### News item

```html
<div class="news-item">
  <div class="news-head">
    <span class="news-title"><a href="{URL}">{HEADLINE}</a></span>
    <span class="badge b-l1" style="margin-left:0">VERIFIED</span>
  </div>
  <div class="news-meta">{SOURCE} · {DATE} · {FACTUAL SUMMARY}</div>
</div>
```

### L4 Analyst view

```html
<div class="analyst">
  <div class="analyst-head">
    <span class="analyst-tag">L4 — Analyst view</span>
    <span class="analyst-disc">Interpretive layer. Based on L1–L3 data above. Verify independently.</span>
  </div>
  <div class="analyst-body">
    <p>{Hedged language: "suggests", "may indicate", "consistent with". No: "credible", "stable", "strong".}</p>
    <p>{Name data gaps explicitly.}</p>
    <p>Worth verifying: {X} before {use case}.</p>
  </div>
</div>
```

### Dig deeper

```html
<div class="dig">
  <div class="dig-title">Dig deeper</div>
  <div class="dig-btns">
    <button class="dig-btn" onclick="navigator.clipboard.writeText('{PROMPT}')">{LABEL}</button>
  </div>
</div>
```

---

## Section order (fixed — always follow)

1. Header
2. Meta bar
3. Layer legend
4. Stats row
5. Legal identity
6. Corporate structure
7. Financial signals (table)
8. Market position
9. Competitive landscape
10. Online presence
11. News & press
12. Legal & regulatory (table)
13. Trust signals
14. L4 Analyst view
15. Dig deeper

Omit a section only if zero data was found. A single data point still gets its own row.
